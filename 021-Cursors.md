# Cursors

## Cursor Nedir?

Cursorlar, veri kümesindeki her bir veriyi adım adım bizlere getiren ve bu şekilde satırsal bazda işlem yapmamızı sağlayan yapılardır. Çalışma şekilleri varsayılan olarak ileri doğru olsada ileri ve geri olmak üzere sırasıyla tüm satırları elde etme usulüne dayanır. İleri doğru okuma işlemi yapan Cursorlar geriye doğru okuma işlemi yapanlardan kat be kat hızlı çalışmaktadırlar. Özetle sorgu neticesinde elde edilen veri kümesinde satır satır gezinerek işlem yapmak için cursor mekanizmasını kullanırız.

```sql
DECLARE @Adi NVARCHAR(MAX), @Soyadi NVARCHAR(MAX), @Unvan NVARCHAR(MAX)
-- Cursor oluşturuluyor.
DECLARE PersonelCursor CURSOR
FOR
-- Cursor'un temsil edeceği veri kümesini getirecek olan sorguyu belirtiliyoruz.
SELECT Adi, Soyadi, Unvan FROM Personeller
-- Cursor açılarak kaynak tüketimi başlatılıyor.
OPEN PersonelCursor
-- Sıradaki veri yakalanıyor ve hafızaya alınıyor. O anki veri "PersonelCursor" tarafından temsil ediliyor ve kolon değerleri ilgili değişkenlere sıralı bir şekilde atanıyor.
FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
-- İşlem başarılı ise @@FETCH_STATUS değişken değeri '0' ise işlem başarılıdır ve bir sonraki kayıt var demektir.
WHILE @@FETCH_STATUS = 0
  BEGIN
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
    -- Sıradaki veriye geçilir ve tekrardan cursor üzerinden kolon değerleri ilgili değişkenlere atanır.
    FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
  END
-- Cursor kapatılıyor.
CLOSE PersonelCursor
-- Tarafımıza ayrılan hafızayı boşaltıyoruz.
DEALLOCATE PersonelCursor
```

"@@FETCH_STATUS" global değişkeni her yeni satır getirildiğinde(FETCH) güncellenir ve bize aşağıdaki dört farklı değer ile durum bildirir;

- 0 FETCH başarılıdır. Sonraki kayıt vardır.
- -1 Kayıt bulunamadı.
- -2 İlk kaydın öncesi yahut son kaydın ötesinde olunduğunu ifade eder. Eksik satır hatasıdır.
- -9 Kayıt getirilemiyor yahut elde edilemiyor.

## Cursor Kaydırma Komutları

Cursor varsayılan olarak sadece ileriye doğru hareket eden bir yönteme sahiptir. Buna FORWARD_ONLY denmektedir. Yukarıdaki örnekte her ne kadar yazmasakta esasında ilgili cursor aşağıdaki gibi FORWARD_ONLY özelliğini ifade edecek şekilde tanımlanabilir.

```sql
DECLARE @Adi NVARCHAR(MAX), @Soyadi NVARCHAR(MAX), @Unvan NVARCHAR(MAX)
DECLARE PersonelCursor CURSOR FORWARD_ONLY
FOR
SELECT Adi, Soyadi, Unvan FROM Personeller
OPEN PersonelCursor
FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
WHILE @@FETCH_STATUS = 0
  BEGIN
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
    FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
  END
CLOSE PersonelCursor
DEALLOCATE PersonelCursor
```

Ayriyetten bizler cursor’ı ileri kaydırdığımız gibi geri de kaydırabilmekte ve hatta ilk ya da son kayda direkt olarak erişebilmekteyiz. Tüm bu işlemleri aşağıdaki FETCH ile kullanacağımız yan komutlar ile gerçekleştirmekteyiz. Tabi bu yan komutların tamamını kullanabilmek için FORWARD_ONLY yerine SCROLL yazılmalıdır;

```sql
DECLARE PersonelCursor CURSOR SCROLL
```

- NEXT bir sonraki kayda/satıra geçer.
- PRIOR; bir önceki kayda/satıra geçer.
- FIRST; ilk kayda/satıra geçer.
- LAST; son kayda/satıra geçer.
- RELATIVE; Cursor’ın bulunduğu kayıttan belirtilen sayı kadar ileriye ya da geriye doğru geçer.
- ABSOLUTE; verilen değer pozitif ise cursor’ın başından, nefatif ise sonundan itibaren değer kadar geçer.

**Not:** SCROLL, NEXT komutu dışındakilerde zorunludur.

```sql
DECLARE @Adi NVARCHAR(MAX), @Soyadi NVARCHAR(MAX), @Unvan NVARCHAR(MAX)
DECLARE PersonelCursor CURSOR SCROLL
FOR
SELECT Adi, Soyadi, Unvan FROM Personeller
OPEN PersonelCursor
FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
FETCH PRIOR FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
FETCH FIRST FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
FETCH RELATIVE 3 FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
FETCH ABSOLUTE -2 FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
CLOSE PersonelCursor
DEALLOCATE PersonelCursor
```

## Cursor Türleri Nelerdir?

### Statik Cursorlar

Static cursorlar, tempdb üzerinde geçici tablo olarak tutulan ve hiç bir şekilde değiştirilemeyen cursorlardır. Bu cursor geçici tablo olacağından dolayı fiziksel tablodaki değişikleri yakalayamayacaktır.

```sql
DECLARE @Adi NVARCHAR(MAX), @Soyadi NVARCHAR(MAX), @Unvan NVARCHAR(MAX)
DECLARE PersonelCursor CURSOR
GLOBAL -- Cursor batch dışında da kullanılabilir.
SCROLL -- Geriye doğru kaydırma yapılabilir.
STATIC -- Cursor tarafından döndürülen sonuç kümesinin, Cursor'un oluşturulduğu anki durumu yansıtmasını isteyebilirsiniz. Bu durumda, STATIC özelliği kullanılır.
FOR
SELECT Adi, SoyAdi, Unvan FROM Personeller
OPEN PersonelCursor
FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
WHILE @@FETCH_STATUS = 0
  BEGIN
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
    FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
  END
CLOSE PersonelCursor
DEALLOCATE PersonelCursor
```

### Keyset-Driven Cursorlar

Keyset-Driven(Anahtar Takımı İle Çalıştırılan) cursorlar, tüm satırları unique olarak tanımlayan veri kümesi sağlamaktadırlar. Kullanıldığı tablo üzerinde unique index olması gerekmektedir. tempdb veritabanında veri kümesi değil, sadece anahtar takımı saklanır.Keyset-Driven cursorlar oluşturuldukları anda bulunan satırlardaki değişikliklere duyarlıdırlar. Lakin oluşturulduktan sonra yeni eklenen satırlara karşı duyarlı değildirler.Bu cursor’ı oluşturduktan sonra yapılan satır ekleme değişiklikleri cursor tarafından yakalanmayacaktır.

```sql
DECLARE @Adi NVARCHAR(MAX), @Soyadi NVARCHAR(MAX), @Unvan NVARCHAR(MAX)
DECLARE PersonelCursor CURSOR
GLOBAL
SCROLL
KEYSET
FOR
SELECT Adi, SoyAdi, Unvan FROM Personeller
OPEN PersonelCursor
FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
WHILE @@FETCH_STATUS = 0
  BEGIN
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
    FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
  END
CLOSE PersonelCursor
DEALLOCATE PersonelCursor
```

### Dinamik Cursorlar

Dinamic cursorlar oluşturulduktan sonra fiziksel tabloda yapılan tüm aktivitelere karşı duyarlı olduklarından dolayı yapılan değişiklikler direkt olarak cursor’a da yansıyacaktır. Tablo temelinde veri duyarlılığına sahip olduklarından dolayı her FETCH komutu çalıştırıldığında SELECT ifadesini WHERE şartı ile birlikte tekrar çalıştıracak şekilde yeniden oluşmaktadırlar. Bundan dolayı tablodaki veri ve sütün genişliğine göre performanslarında olası olumsuz değişimler söz konusu olabilmektedir.

Diğer cursor’lara nazaran fiziksel olan tempdb üzerinde çalışmaktansa RAM(geçici bellek)de çalışmakta ve böylece fiziksel diske nazaran daha hızlı işlem görmektedirler. İşte bundan dolayı özellikle küçük boyutlu verilerde veri duyarlılığıyla birlikte performanslı bir çalışma sergilerler.

```sql
DECLARE @Adi NVARCHAR(MAX), @Soyadi NVARCHAR(MAX), @Unvan NVARCHAR(MAX)
DECLARE PersonelCursor CURSOR
DYNAMIC --Cursor'a dinamik özellik kazandırıyoruz.
SCROLL
GLOBAL
FOR
SELECT Adi, SoyAdi, Unvan FROM Personeller
OPEN PersonelCursor
FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
WHILE @@FETCH_STATUS = 0
  BEGIN
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
    FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
  END
CLOSE PersonelCursor
DEALLOCATE PersonelCursor
```

## Cursor’da Sütun Güncelleme(FOR UPDATE)

Ana tabloda cursor içerisinde kullanılan kolonlardan herhangi birinde bir güncelleme söz konusu olursa eğer bunu FOR UPDATE ile belirterek aşağıdaki gibi güncellemeyi ilgili cursor’a yansıtabilmekteyiz.

Burada sadece belirtilen sütunların güncelleneceğine dikkatinizi çekerim. Aksi taktirde belirtilmeyen kolonlar read only olacak ve herhangi bir müdahale edildiğinde hata ile karşılaşılacaktır.

```sql
DECLARE @Adi NVARCHAR(MAX), @Soyadi NVARCHAR(MAX), @Unvan NVARCHAR(MAX)
DECLARE PersonelCursor CURSOR
FOR
SELECT Adi, SoyAdi, Unvan FROM Personeller
FOR UPDATE OF Adi, Soyadi
OPEN PersonelCursor
FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
WHILE @@FETCH_STATUS = 0
  BEGIN
    UPDATE Personeller SET Adi = 'Hüseyin', SoyAdi = 'Kamburoğulları' WHERE CURRENT OF PersonelCursor
    PRINT @Adi + ' ' + @Soyadi + ' ' + @Unvan
    FETCH NEXT FROM PersonelCursor INTO @Adi, @Soyadi, @Unvan
  END
CLOSE PersonelCursor
DEALLOCATE PersonelCursor
```
