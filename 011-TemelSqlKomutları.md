
# Temel Sql Komutları

## Top Komutu
SELECT TOP komutu, çağırılacak verilerin sayısını belirtmek için kullanılır. Tüm veritabanı sistemleri SELECT TOP komutunu desteklemez. MySQL sınırlı sayıda veri seçmek için LIMIT komutunu desteklerken, Oracle ROWNUM kullanır.

```sql
USE Northwind
SELECT TOP 3 * FROM Personeller
```

### Top Komutu İle Update İşlemi

```sql
USE Db_Education
UPDATE TOP(2) DenemePersoneller SET Isım = 'Boş' 
```

### Top Komutu İle Delete İşlemi

```sql
DELETE TOP(5) FROM DenemePersoneller
```

## Distinct Komutu

Bir kolondaki benzer olan verileri teke indirmemizi sağlayan komuttur. 

```sql
USE Northwind
SELECT Sehir FROM Personeller
SELECT DISTINCT Sehir FROM personeller
SELECT COUNT(DISTINCT Sehir) FROM personeller
```

## Group By Komutu

Genel Formül: SELECT Listelenecek_alanlar, fonksiyonlar FROM Tablo_adı WHERE şart veya şartlar GROUP BY Gruplandırılacak_alanlar

Kolonlardaki satırların gruplanmasını sağlar. Group by en çok aggregate fonksiyonlarla(max, min, avg, count, sum) beraber kullanılır. Fonkisiyonlar kullanılmak zorunda değildir. Group by da "Listelenecek alanlar", "Gruplandırılıcak alanlar" içinde olmak zorundadır. Gruplandırılıcak alanların hepsi listelenmek zorunda değildir.

```sql
-- Örnek 1
SELECT * FROM Urunler
SELECT KategoriID , COUNT(*) FROM Urunler GROUP BY KategoriID

-- Örnek 2
SELECT KategoriID FROM Urunler GROUP BY KategoriID

-- Örnek 3
SELECT KategoriID , TedarikciID , COUNT(*) FROM Urunler GROUP BY KategoriID , TedarikciID

-- Örnek 4
SELECT * FROM Satislar
SELECT PersonelID , SUM(NakliyeUcreti) FROM Satislar GROUP BY PersonelID
```

Group By İşleminde Where Şartı Yazma: Where komutu group by komutundan önce yazılır. Where komutu kolonlardaki şartlar için kullanılır.

```sql
SELECT KategoriID , COUNT(*) FROM Urunler WHERE KategoriID > 5 GROUP BY KategoriID
```

## Having Komutu

Group by komutundan sonra yazılır ve her zaman group by ile birlikte kullanılır. Having komutu aggregate fonksiyonlarında ki şartlar için yazılır.

```sql
-- Örnek 1
SELECT KategoriID , COUNT(*) FROM Urunler WHERE KategoriID < 5 GROUP BY KategoriID HAVING COUNT(*) > 10

-- Örnek 2
SELECT PersonelID , SUM(NakliyeUcreti) FROM Satislar GROUP BY PersonelID HAVING SUM(NakliyeUcreti) > 5000
```

## With Rollup Komutu

Group By ile gruplandırılmış veri kümesinde ara toplam alınmasını sağlar. 

```sql
-- Örnek 1 (10252 numaralı satış id'lerindeki bütün ürünlerin toplam miktarını toplayan ve bunu sorguya yeni bir satır ekleyerek gösteren bir örnektir.)
-- Aşağıdaki örnekte aslında 10252 numaralı satış id'lerine karşılık gelen SUM(Miktar) değerlerini topayıp, yeni satır olarak sonuca ekledi.
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH ROLLUP WHERE SatisID = 10252

-- Örnek 2 (With Rollup komutunun Having komutu ile kullanılması)
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH ROLLUP HAVING SUM(Miktar) > 100
```

## With Cube Komutu

Group By ile gruplandırılmış veri kümesinde teker teker toplam alınmasını sağlar. With Rollup ile aslında SatisID alanına göre SUM(Miktar) değerleri toplanıp yeni satır olarak eklendi. With Cube ile aslında UrunID alanına göre SUM(Miktar) değerleri toplanıp yeni satır olarak eklendi.

```sql
-- Örnek 1
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH CUBE

-- Örnek 2 (With Rollup komutunun Having komutu ile kullanılması)
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH CUBE HAVING SUM(Miktar) > 100
```

## Order By Komutu

Kayıtları belirtilen alanda büyükten küçüğe veya küçükten büyüğe göre sıralar. Defaul olarak küçükten büyüğe sıralanır. (ASC)

1. ASC (Ascending) --> Küçükten büyüğe sıralama
2. DESC (Descending) --> Büyükten küçüğe sıralama

```sql
SELECT * FROM [Satis Detaylari] ORDER BY UrunID

SELECT * FROM [Satis Detaylari] ORDER BY UrunID ASC

SELECT * FROM [Satis Detaylari] ORDER BY UrunID DESC

SELECT UrunID, BirimFiyati FROM [Satis Detaylari] ORDER BY BirimFiyati DESC

SELECT * FROM [Satis Detaylari] ORDER BY İndirim ASC , Miktar DESC

-- ORDER BY Komutunda WHERE Şartı Yazma
SELECT * FROM [Satis Detaylari] WHERE Miktar >= 100 ORDER BY UrunID ASC
```

## With Ties komutu

WITH TIES, TOP ve ORDER BY ile birlikte kullanılır. ORDER BY ile sıralanan sonuçlarda son kayıt ile aynı değerde olan kayıtların da listelenmesini sağlar. Bu durumda sonuç belirtilen n sayısından daha fazla olabilir. WITH TIES sadece ORDER BY ile birlikte kullanılmaktadır.

```sql
-- Aşağıdaki yapıda “Satis Detayları” adlı tablodan ilk 6 veri gelecektir.Ancak bu verilerden sonuncu olanı “10250” nolu kayıtın iki adeti dışarda kalmaktadır.Bu ikisinide getirmek için “WITH TIES” yapısını kullanmaktayız. With Ties order by ile sıralanmış kolonda, en son kayda bakar, eğer onun altında aynı nolu kayıt varsa onu da getirir. Bu yapıyı kullanabilmek için tek şart order by kullanılmalıdır.

SELECT TOP 6 * FROM [Satis Detaylari] 

SELECT TOP 6 WITH TIES * FROM [Satis Detaylari] 
```

## Toplu Kullanım

```sql
SELECT KategoriID, TedarikciID, COUNT(*) FROM Urunler WHERE KategoriID < 5 
GROUP BY KategoriID, TedarikciID HAVING COUNT(*) < 4 ORDER BY KategoriID DESC
```



 








