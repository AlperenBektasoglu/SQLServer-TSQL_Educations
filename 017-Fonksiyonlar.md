# Fonksiyonlar

## Fonksiyon ile Store Procedure Arasındaki Farklar

1. Stored Procedure hem input hem de output parametrelerine sahip olabilir, ancak bir function yalnızca input parametrelerine sahip olabilir.
2. Stored Procedure içinde DML (INSERT/UPDATE/DELETE) ifadelerini kullanabiliriz. Ancak bir function içinde DML ifadelerini kullanamayız.
3. Stored procedure’ler CRUD (Create/Read/Update/Delete) işlemlerinin hepsini yapabilir, fakat Function‘lar sadece Select ile kullanılabilir.
4. Bir Stored Procedure içinde Try-Catch Bloğu kullanabiliriz ancak function içinde Try-Catch bloğu kullanamayız.
5. Stored procedure’ler transaction yapısını destekler, fakat function‘lar transaction yapısını desteklemez.
6. Stored Procedure ile döndürülen sonuç kümesi bir JOIN işlemine dahil edilemez. Function ile döndürülen sonuç kümesi JOIN işleminde kullanılabilir.
7. Stored Procedure’ler SQL ifadelerinde WHERE/HAVING/SELECT bölümünün herhangi bir yerinde kullanılamaz. Ancak bir function’ı herhangi bir yerde kullanabiliriz.
8. Stored Procedure 0 veya n tane değer döndürebilir (maksimum 1024). Ancak bir fonksiyon yalnızca 1 değer döndürebilir ve bu değer zorunludur.
9. Stored procedure’ler function tarafından çağrılamaz, fakat function‘lar Stored procedure tarafından çalıştırılabilir.

## Fonksiyon Tipleri

T-Sql de iki tip fonksiyon vardır:

1. Scalar Fonksiyonlar: Geriye istediğimiz bir tipte değer döndüren fonksiyonlardır. (Klasör: Scalar-valued Functions)
2. Inline Fonsiyonlar: Geriye tablo döndüren fonksiyonlardır. (Klasör: Table-valued Functions)

## Scalar Fonksiyonlar

```sql
USE Northwind
GO

-- Örnek 1
CREATE FUNCTION BuyukHarfYap (@gelenDeger VARCHAR(10))
RETURNS VARCHAR(10)
AS
BEGIN
RETURN UPPER(@gelenDeger)
END

-- Not: Fonksiyonlar çağırılırken şema isimleri ile çağrılır.
SELECT dbo.BuyukHarfYap('alperEN')
PRINT dbo.BuyukHarfYap('alperEN')
SELECT dbo.BuyukHarfYap(Adi) , dbo.BuyukHarfYap(SoyAdi)  FROM Personeller

-- Örnek 2
CREATE FUNCTION SehireGöreKolonSay (@gelendeger VARCHAR(MAX))
RETURNS INT
AS
BEGIN
DECLARE @x INT
SELECT @x = COUNT(Sehir) FROM Personeller WHERE Sehir = @gelendeger
RETURN @x
END

SELECT dbo.SehireGöreKolonSay('London')
PRINT dbo.SehireGöreKolonSay('London')
```

## Inline Fonksiyonlar

Inlıne fonksiyonların scalar fonksiyonlardan farkı tablo döndürmesi ve BEGIN-END komutlarının olmamasıdır.

```sql
-- Örnek 1
CREATE FUNCTION UlkeyeGoreBul (@gelendeger VARCHAR(20))
RETURNS TABLE
AS
RETURN SELECT * FROM Personeller WHERE Ulke = @gelendeger

SELECT * FROM dbo.UlkeyeGoreBul('UK')
SELECT Adi , SoyAdi , Ulke FROM dbo.UlkeyeGoreBul('UK')

-- Örnek 2
CREATE FUNCTION UlkeyeGoreBul_1 (@gelendeger VARCHAR(20))
RETURNS TABLE
AS
RETURN (SELECT * FROM Personeller WHERE Ulke = @gelendeger)

SELECT * FROM dbo.UlkeyeGoreBul_1('UK')
SELECT Adi , SoyAdi , Ulke FROM dbo.UlkeyeGoreBul_1('UK')
```

## Alter Komutu İle Fonksiyon Yapısını Değiştirme

```sql
ALTER FUNCTION BuyukHarfYap (@gelenDeger_1 VARCHAR(10), @gelenDeger_2 VARCHAR(10))
RETURNS VARCHAR(20)
AS
BEGIN
RETURN UPPER(@gelenDeger_1 + ' ' + @gelenDeger_2)
END

SELECT dbo.BuyukHarfYap('alperEN','BEKtaşoğlu')
PRINT dbo.BuyukHarfYap('alperEN','BEKtaşoğlu')
```

## Fonsiyon Silme

```sql
DROP FUNCTION dbo.BuyukHarfYap
```
