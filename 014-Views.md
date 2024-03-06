
# Views

View, bir veya birden fazla tablodan değişik sorgularla çekilmekte olan bilgilerin oluşturduğu sanal sonuç tablolarıdır. View'ların kullanım amaçları aşağıda verilmiştir:
1. Bir kullanıcının bir tablo içerisindeki herşeyi görmesinin istenmediği durumlarda kullanılır.
2. Bir kullanıcının çok miktarda veri bulunduran bir tablo üzerinde sorgulama yapması yerine view yapısı kullanılabilir. Sorgu işlemlerini daha kolaylaştırmak/hızlandırmak için bu verilerin bir bölümü üzerinde çalışması istendiğinde kullanılır.
4. Farklı ve karmaşık tablolarda tutulan verilerin tek ve sade bir şekilde görülmesi gerektiğinde view yapısı kullanılabilir.
5. Kompleks sorguları basitleştirmek gerektiğinde kullanılabilir.

View içerisinde yapılamayacak işlemler aşağıda verilmiştir: 
1. İsimsiz bir kolon kullanılamaz. (sql aggregate fonksiyon kullanımından sonra kolon ismi boş gelir.)
2. Stored procedure yapısı gibi, view içerisinde parametre gönderilemez.
3. View yapısı içerisinde dml kodları (insert into, update, delete) kullanılamaz.
4. View yapısı içerisinde, sadece select ile başlayan ifadeler kullanılabilir.
5. View içerisinde order by (sıralama) fonksiyonu kullanılamaz.
6. View'ı oluşturan kod bloğunda order by, compute, compute by, into gibi ifadeler bulunamaz.

## View Oluşturma

```sql
USE Northwind

-- Örnek 1
CREATE VIEW OrnekView_1
AS
SELECT Adi, SoyAdi, Unvan FROM Personeller

SELECT * FROM OrnekView_1

-- Örnek 2
CREATE VIEW OrnekView_2
(Isim, Soyisim, TakmaAd)
AS
SELECT Adi, SoyAdi, Unvan FROM Personeller

SELECT * FROM OrnekView_2

-- Örnek 3
CREATE VIEW OrnekView_3
AS
SELECT Adi Isim, SoyAdi Soyisim, Unvan Takma_Ad FROM Personeller

SELECT * FROM OrnekView_3

-- Örnek 4 (Hata verir çünkü view'ı oluşturan kod bloğunda order by, compute, compute by, into gibi ifadeler bulunamaz.)
-- CREATE VIEW OrnekView_4
-- AS
-- SELECT * FROM Personeller
-- ORDER BY BagliCalistigiKisi DESC

-- Örnek 5 (Birden Fazla Tablodan Kayıt Çeken View Oluşturma)
CREATE VIEW OrnekView_5 
AS
SELECT SirketAdi, MusteriAdi, UrunAdi, UrunID FROM Tedarikciler T INNER JOIN Urunler U ON T.TedarikciID = U.TedarikciID
```

## View'in Yapısını Değiştirme

```sql
SELECT * FROM OrnekView_1

ALTER VIEW OrnekView_1
AS 
SELECT Adi FROM Personeller
```

## Varolan View’in Kodunu Görme

```sql
sp_helptext OrnekView_1
```

## Tanımlı View Listesini Görme

```sql
SELECT * FROM sys.views
```

##  View Kodunun Şifrelenmesi / With Encryption Komutu

```sql
CREATE VIEW OrnekView_6 WITH ENCRYPTION
AS
SELECT * FROM Personeller WHERE BagliCalistigiKisi BETWEEN 1 AND 3

SELECT * FROM OrnekView_6 -- Sonucu Görürsün.
sp_helptext OrnekView_6 -- Sorguyu göremezsin.
```

**Not:** Şifrelenmiş bir view, alter komutu ile güncellenirken her seferinde with encryption ifadesi kullanılmalıdır. Kullanılmaz ise şifrelemeden vazgeçildiği şeklinde düşünülerek çalıştırılır.

```sql
ALTER VIEW OrnekView_6 WITH ENCRYPTION
AS
SELECT Adi FROM Personeller
```

## View’in Silinmesi

```sql
DROP VIEW OrnekView_6
```

## View’daki Tabloya Kayıt Ekleme / Güncelleme / Silme

Viewler bünyelerinde veri tutmadıklarından dolayı ilgili değişiklikler doğrudan base tablo üzerinde gerçekleşirler. Bir view, veri gireceği base tablodaki identity, not null, constraint veya indekslere takılmamalıdır.

```sql
CREATE VIEW OrnekView_7
AS
SELECT Adi, SoyAdi, Unvan FROM Personeller

INSERT OrnekView_7 VALUES('Alperen', 'Bektaşoğlu', 'DBM')

UPDATE OrnekView_7 SET Unvan = 'Database Manager' WHERE Adi = 'Alperen'

DELETE FROM OrnekView_7 WHERE Adi = 'Alperen'
```

## With Check Option Komutu

Insert ve update komutlarında şarta uygunluğu test eder. Şartı belirleyen faktör view oluşturulurken koyulan where komutudur.

```sql
CREATE VIEW OrnekView_8
AS
SELECT Adi, SoyAdi FROM Personeller WHERE Adi LIKE 'a%'
WITH CHECK OPTION

SELECT * FROM OrnekView_8

INSERT OrnekView_8 VALUES('Alperen','Bektaşoğlu')
INSERT OrnekView_8 VALUES('Tuncel','Kurtiz')
UPDATE OrnekView_8 SET Adi = 'Ezgi' WHERE SoyAdi = 'Bektaşoğlu'
```

## View’in Adını Değiştirme

```sql
-- 1. Yol
Sp_rename 'OrnekView_8', 'OV8'

-- 2. Yol
EXEC Sp_rename 'OV8', 'OV_8'

-- 3. Yol
EXECUTE Sp_rename 'OV_8', 'OrnekView_8'
```
