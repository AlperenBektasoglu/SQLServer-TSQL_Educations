
# Temel Sql Komutları

## Top Komutu
SELECT TOP komutu, çağırılacak verilerin sayısını belirtmek için kullanılır. Tüm veritabanı sistemleri SELECT TOP komutunu desteklemez. MySQL sınırlı sayıda veri seçmek için LIMIT komutunu desteklerken, Oracle ROWNUM kullanır.

```sql
USE Northwind
SELECT TOP 3 * FROM Personeller

USE Db_Education
UPDATE TOP(2) DenemePersoneller SET Isım = 'Boş' 
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

Kolonlardaki satırların gruplanmasını sağlar.

Genel Formül: SELECT [kolonadı] , [aggregate_fonksiyonu] FROM [tabloadı] GROUP BY [kolonadı]

```sql
-- Örnek 1
SELECT * FROM Urunler
SELECT KategoriID , COUNT(*) FROM Urunler GROUP BY KategoriID

-- Örnek 2
SELECT KategoriID , TedarikciID , COUNT(*) FROM Urunler GROUP BY KategoriID , TedarikciID

-- Örnek 3
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

## Toplu Kullanım

```sql
SELECT KategoriID, TedarikciID, COUNT(*) FROM Urunler WHERE KategoriID < 5 
GROUP BY KategoriID, TedarikciID HAVING COUNT(*) < 4 ORDER BY KategoriID DESC
```



 








