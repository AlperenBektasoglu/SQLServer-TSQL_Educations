# Programlama Yapıları

## Sql'de Değişken Tanımlama

Değişken tanımlamayı ve değişkene değer atamayı birlikte çalıştırmalısız!

```sql
USE Northwind

-- Değişken tanımlama örnekleri
DECLARE @x INT
DECLARE @a INT , @b NVARCHAR(5)

-- Değişken tanımlamayı ve değişkene değer atamayı birlikte çalıştırmalısız!
DECLARE @y INT = 100
PRINT @y

SET Komutu Kullanarak Değişkene Değer Atama
DECLARE @x
SET @x = 400
PRINT @x

DECLARE @x INT = 200
PRINT @x -- Çıktı: 200
SET @x = 400
PRINT @x -- Çıktı: 400

-- Select komutu kullanarak değişkene değer atama
DECLARE @enYuksekFiyat  MONEY
SELECT @enYuksekFiyat = 23754.78
PRINT @enYuksekFiyat

-- Sorgu sonucu gelen verileri değişkenlere atama
-- Kurallar:
-- 1. Sorgu sonucu gelen satır sayısı 1 olmalıdır.
-- 2. Kolonlardaki veri tipleri ne ise değişkenlerinde veri tipi o olmalıdır.

DECLARE @Isim NVARCHAR(MAX) , @SoyIsim NVARCHAR(MAX)
SELECT @Isim = Adi , @SoyIsim = SoyAdi FROM Personeller WHERE PersonelID = 7

SELECT @Isim , @SoyIsim
SELECT 'İlgili personelin adı - Soyadı: ' + @Isim + ' ' + @SoyIsim
```

## Birleşik Operatörler

Birleşik operatörler, bir değişkenin değerini bir değer ile işleme sokup; geri tekrar aynı değişkenin üzerine yazan operatörlerdir.

```sql
DECLARE @sayac int = 0
SET @sayac += 7 -- @sayac = @sayac + 7
SELECT @sayac As Arti_7 -- Çıktı: 7
SET @sayac -= 2 -- @sayac = @sayac - 7
SELECT @sayac As Eksi_2 -- Çıktı: 5
SET @sayac *= 10 -- @sayac = @sayac * 7
SELECT @sayac As Carpı_10 -- Çıktı: 50
SET @sayac /= 5 -- @sayac = @sayac / 7
SELECT @sayac As Bolü_5 -- Çıktı: 10
SET @sayac %= 8 -- @sayac = @sayac % 7
SELECT @sayac As Mod_8 -- Çıktı: 2
```








