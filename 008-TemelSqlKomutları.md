
# Temel Sql Komutları

## Select Komutu

Seç ve getir işlemi yapar.

```sql
USE Northwind

SELECT 3    
SELECT 'Alperen'
PRINT 'Alperen'
SELECT 'Alperen' , 'Bektaşoğlu' , 21 , 42
SELECT * FROM Personeller
SELECT Adi , SoyAdi FROM Personeller
```

## Alias(Takma Ad) Kulanımı

Sütuna isim atama işlemi yapar.'as' ifadesinin kullanılması zorunlu değildir.

```sql
SELECT 'Alperen' as Ad , 'Bektaşoğlu' Soyad
SELECT Adi isim , SoyAdi Soyisim FROM Personeller

-- Boþluk karakteri olunca Alias atama
SELECT 'Alperen Bektaþoðlu' [Ad Soyad]

-- Boşluk karakteri içeren tabloyu sorgulama
SELECT * FROM [Satis Detaylari]
```

## Kolon Birleştirme

```sql
-- Aynı tipteki kolonları birleştirme
SELECT Adi + ' ' + SoyAdi  AdSoyad FROM Personeller

-- Farklı tipteki kolonları birleştirme
SELECT Adi + ' ' + IseBaslamaTarihi  FROM Personeller --Hatalı işlem
SELECT Adi + ' ' + CONVERT(nvarchar , IseBaslamaTarihi) FROM Personeller
SELECT Adi + ' ' + CAST(IseBaslamaTarihi AS nvarchar) FROM Personeller
```

## Tablo Oluştururken As komutu kullanma

```sql
USE Db_Education

CREATE TABLE Hesaplama(
BirinciSayi INTEGER,
IkinciSayi INTEGER,
Ortalama AS (BirinciSayi + IkinciSayi)/2,
Toplam AS (BirinciSayi + IkinciSayi)
)
```

## Select Komutunda Where Şartı

```sql
-- Personeller tablosunda şehri London olan verileri listele.
SELECT * FROM Personeller WHERE Sehir = 'London'

-- Personeller tablosunda bağlı çalıştığı kşi saıyısı 5 ten küçük olanları listele.
SELECT * FROM Personeller WHERE BagliCalistigiKisi < 5

-- Doğum günü ayın 29 u olmayan personelleri listele.
SELECT * FROM Personeller WHERE DAY(DogumTarihi) <> 29

-- Farklı Bir Select Kullanımı 
SELECT * FROM ( VALUES ('ali',70),('veli',60),('can',50) ) AS C(isim, agirlik) WHERE agirlik < 60

------- MANTIKSAL OPERATÖRLERİN KULLANIMI  -------

-- AND Operatörü
-- Personeller tablosunda şehri London ve ülkesi UK olanları listele.
SELECT * FROM Personeller WHERE Sehir = 'London' AND Ulke = 'UK'

-- OR Operatörü
-- Personeller tablosunda ünvan eki 'mr.' veya şehiri 'Seattle'  olanları listele.
SELECT * FROM Personeller WHERE UnvanEki = 'Mr.' OR Sehir = 'Seattle'

-- NOT Operatörü
-- Personeller tablosunda þehri London olmayan verileri listele.
SELECT * FROM Personeller WHERE NOT(Sehir = 'London')

------- CONDITIONS (KOŞULLAR) KULLANIMI -------

-- BETWEEN Kullanımı
SELECT * FROM Personeller WHERE YEAR(DogumTarihi) > 1950 AND YEAR(DogumTarihi) <1965
SELECT * FROM Personeller WHERE YEAR(DogumTarihi) BETWEEN 1950 AND 1965

-- IN Kullanımı
SELECT * FROM Personeller WHERE Sehir = 'Seattle' OR Sehir = 'Tacoma' OR Sehir = 'Kirkland'
SELECT * FROM Personeller WHERE Sehir IN('Seattle' , 'Tacoma' , 'Kirkland')

```

### Where Bloğunda Kullanılan Bağlaçaların İşlem Önceliği Sıralaması

Where Bloğundaki işlem öncelik sıralaması:
1. Parantez içi işlemler
2. Tekil bazda tüm operatörler
3. NOT işlemi
4. AND işlemi
5. OR işlemi

```sql
USE Db_Education

CREATE TABLE Personeller(
PerNo INTEGER,
Ad NVARCHAR(20),
Gorev NVARCHAR(20),
Ucret INTEGER
)

SELECT Perno FROM Personeller WHERE Gorev = 'memur' OR Gorev = 'Sef' AND  Ucret > 3000
/* {1,2,3,5,8} OR {4,7} AND {4,6,7}
   {1,2,3,5,8} OR {4,7}
   {1,2,3,4,5,7,8}
*/

SELECT Perno FROM Personeller WHERE NOT(Gorev IN ('Mudur','Sef')) AND Ad LIKE '%e%' OR ( Ucret < 3000 AND PerNo < 5 ) 
/* NOT({4,5,7}) AND {4,5,8,9} OR ({1,2,5,8,9,} AND {1,2,3,4})
   {1,2,3,6,8,9}  AND  {4,5,8,9}  OR  {1,2}
   {8,9}  OR  {1,2}
   {1,2,8,9}
*/
```

## Select Komutunda Like Şartı

```sql
USE Northwind

------------ % (Genel Önemli Değil Operatörü) ------------

-- İsmin baş harfi 'j' olan personellerin adını ve soyadını yazdır.
SELECT adi , soyadi FROM Personeller WHERE adi LIKE 'j%'

-- İsmin son harfi 'y' olan personellerin adını ve soyadını yazdır.
SELECT adi , soyadi FROM Personeller WHERE adi LIKE '%y'

-- İsmin son 3 harfi 'ert' olan personeli göster.
SELECT * FROM Personeller WHERE adi LIKE '%ert'

-- İsmin ilk harfi 'r' son harfi 't' olan personeli göster.
SELECT * FROM Personeller WHERE adi LIKE 'r%t'
SELECT * FROM Personeller WHERE adi LIKE 'r%' AND adi LIKE '%t' 

-- İsmin baş harfi 'n' olan ve içinde 'an' geçen personeli göster.
SELECT * FROM Personeller WHERE adi LIKE 'n%an%'

------------ _ (Özel Önemli Deðil Operatörü) ------------

-- İsmin ilk harfi 'a', ikinci harfi önemli değil, üçüncü harfi 'd' olan personeli göster.
SELECT * FROM Personeller WHERE adi LIKE 'a_d%'

-- İsmin ilk harfi 'a' , ikinci ve üçüncü harfleri önemli deil ,dördüncü harfi 'r' olan personeli göster.
SELECT * FROM Personeller WHERE adi LIKE 'a__r%'

------------ [] (Ya da Operatörü) ------------

-- İsmin baş harfi 'n' yada 'm' yada 'r' olan personelleri listele.
SELECT * FROM Personeller WHERE adi LIKE '[nmr]%'

-- İsmin içerisinde 'a' yada 'i' içeren personelleri listele.
SELECT * FROM Personeller WHERE adi LIKE '%[ai]%'

------------ [*-*] (Alfabetik Operatörü) ------------

--İsmin baş harfi 'a' ile 'k' arasındaki personelleri yazdır.
SELECT * FROM Personeller WHERE adi LIKE '[*a-k*]%'

------------ [^ifade] (Deðil Operatörü) ------------

-- İsmin baş harfi 'an' olmayan personelleri listele.
SELECT * FROM Personeller WHERE adi LIKE '[^an]%'
```

###  Like Sorgusunda Escape(Kaçış) Karakteri

Like sorgularında kullandığımız _ , %  gibi özel ifadeler verilerimiz içerisinde geçiyorsa bu problemi iki şekilde çözeriz:
1. [] operatörü ile
2. escape komutu ile

```sql
SELECT * FROM Personeller WHERE adi LIKE '[_]%'
SELECT * FROM Personeller WHERE adi LIKE '+_%' ESCAPE '+'
SELECT * FROM Personeller WHERE adi LIKE 'a_%' ESCAPE 'a'
SELECT * FROM Personeller WHERE SoyAdi LIKE '^%%' ESCAPE '^'
```

## Top Kullanımı
SELECT TOP komutu, çağırılacak verilerin sayısını belirtmek için kullanılır. Tüm veritabanı sistemleri SELECT TOP komutunu desteklemez. MySQL sınırlı sayıda veri seçmek için LIMIT komutunu desteklerken, Oracle ROWNUM kullanır.

```sql
SELECT TOP 100 * FROM Personeller
```


