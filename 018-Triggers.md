
# Triggers(Tetikleyiciler)

Triggerlar iki ye ayrılır:
1. DDL Trigger
2. DML Trigger

Triggerlar çalışma mantığı açısından da ikiye ayrılırlar:
1. İşlem sonucunda çalışan triggerlar  -- AFTER/FOR
2. İşlemin yerine çalışan triggerlar   -- INSTEAD OF 

## DDL Trigger

Create, alter ve drop işlemleri sonucunda devreye giren yapılardır. Bu işlemler sonucunda veya sürecinde devreye girerler. 

```sql
-- Kullanım Protitipi:
-- CREATE TRIGGER [ Trigger Adı ]
-- ON DATABASE
-- AFTER/FOR drop_table, alter_table, create_function, create_procedure, drop_procedure ...
-- AS
-- [Çalışması İstenen Kodlar] 
```
**Not:** INSTEAD OF yapısı DDL Triggerlarda kullanılmaz.

**Not:** DDL Triggerlara ilgili veritabanının Programmability -> Databese Triggers klasöründen ulaşabilirsiniz.

```sql
CREATE TABLE LogTablosu(
Id INT PRIMARY KEY IDENTITY(1,1),
Rapor NVARCHAR(MAX)
)

-- Örnek 1
CREATE TRIGGER Tablo_Silme_Engelleme_Trigger_1
ON DATABASE
AFTER drop_table
AS
PRINT 'Bu işlem Tablo_Silme_Engelleme_Trigger_1 triggerı tarafından engellenmiştir.'
ROLLBACK

DROP TABLE LogTablosu

-- Örnek 2
CREATE TRIGGER Tablo_Silme_Engelleme_Trigger_2
ON DATABASE
FOR drop_table
AS
PRINT 'Bu işlem Tablo_Silme_Engelleme_Trigger_2 triggerı tarafından engellenmiştir.'
ROLLBACK

DROP TABLE LogTablosu
```

## DML Trigger

Bir tabloda insert, update ve delete işlemleri gerçekleştirildiğinde devreye giren yapılardır. Bu işlemler sonucunda veya sürecinde devreye girerler.

Inserted Table: Eğer bir tabloda insert işlemi yapılıyor ise arka planda işlemler, ilk önce RAM'de oluşturulan inserted isimli bir tablo üzerinde yapılır. Eğer işlemde bir problem yoksa inserted tablosundaki veriler fiziksel tabloya Insert edilir. İşlem bittiği zaman RAM'de oluşturulan bu inserted tablosu silinir. Bu anlatılan insert komutunun çalışma prensibidir.

Deleted Table: Eğer bir tabloda delete işlemi yapılıyor ise arka planda işlemler, ilk önce RAM'de oluşturulan deleted isimli bir tablo üzerinde yapılır. Eğer işlemde bir problem yoksa deleted tablosundaki veriler fiziksel tablodan silinir. İşlem bittiği zaman RAM'de oluşturulan bu deleted tablosu silinir. Bu anlatılan delete komutunun çalışma prensibidir.

**Not:** Eğer bir tabloda update işlemi yapılıyor ise RAM'de updated isimli bir tablo OLUŞTURULMAZ!SQL Server'da ki update mantığı önce silme(Delete), sonra eklemedir(Insert). Eğer bir tabloda update işlemi yapılıyor ise arka planda, RAM'de hem deleted hem de inserted tabloları oluşturulur ve işlemler bunlar üzerinden yapılır.

**Not:** Update işlemi yaparken; güncellenen kaydın orjinali deleted tablosunda, güncellendikten sonraki hali ise Inserted tablosunda bulunmaktadır.

**Not:** Deleted ve inserted tabloları, ilgili sorgu sonucu oluştukları için o sorgunun kullandığı kolonlara da sahip olur. Böylece deleted ve inserted tablolarından select sorgusu yapmak mümkündür.

```sql
-- Kullanım Protitipi:
-- CREATE TRIGGER [ Trigger Adı ]
-- ON [ İşlem Yapılacak Tablo Adı ]
-- AFTER/FOR INSERT, UPDATE, DELETE
-- AS
-- [Çalışması İstenen Kodlar]  -- Kod kısmını BEGIN-END komutları içine aladabilirsin, almayadabilirsin.
```

```sql
USE Northwind

-- Örnek 1
CREATE TRIGGER OrnekTrıgger_1
ON Personeller
AFTER INSERT -- AFTER yerine FOR da yazabilirsiniz.
AS
SELECT * FROM Personeller

INSERT Personeller(Adi, SoyAdi) VALUES ('Alperen','Bektaşoğlu')

-- Örnek 2
CREATE TRIGGER OrnekTrıgger_2
ON Personeller
AFTER UPDATE 
AS
PRINT 'Güncelleme işlemi başarılı...'
INSERT Kategoriler(KategoriAdi) VALUES ('Yeni Ürün')

UPDATE Personeller SET Adi = 'Gökçen' WHERE SoyAdi = 'Bektaşoğlu'

-- Örnek 3
CREATE TRIGGER OrnekTrıgger_3
ON Personeller
AFTER DELETE 
AS
PRINT 'Silme işlemi başarılı...'
SELECT * FROM Personeller

DELETE FROM Personeller WHERE SoyAdi = 'Bektaşoğlu'

-- Örnek 4 (Multiple Actions Trigger)
CREATE TRIGGER OrnekTrıgger_4
ON Personeller
AFTER INSERT, UPDATE, DELETE
AS
PRINT 'işlem başarılı...'

INSERT Personeller(Adi, SoyAdi) VALUES ('Alperen','Bektaşoğlu')
UPDATE Personeller SET Adi = 'Gökçen' WHERE SoyAdi = 'Bektaşoğlu'
DELETE FROM Personeller WHERE SoyAdi = 'Bektaşoğlu'

-- Örnek 5 (Personeller tablosuna bir kayıt eklendiği zaman, eklenen kaydın adı, soyadı, kim tarafından ve hangi tarihte eklendiğini başka bir tabloya kayıt eden trigger'ı yazınız.) (Log Tablosu Örneği - 1)
CREATE TABLE LogTablosu(
Id INT PRIMARY KEY IDENTITY(1,1),
Rapor NVARCHAR(MAX)
)

CREATE TRIGGER Log_Personel_Ekleme_Trigger
ON Personeller
AFTER INSERT
AS
DECLARE @Ad NVARCHAR(MAX), @Soyad NVARCHAR(MAX)
SELECT @Ad = Adi, @Soyad = SoyAdi FROM inserted
INSERT LogTablosu VALUES ('Adı ve soyadı ' + @Ad + ' ' + @Soyad + ' olan personel, ' + SUSER_NAME() + ' tarafından ' + CAST(GETDATE() AS nvarchar(MAX)) + ' tarihinde eklenmiştir.')

INSERT Personeller(Adi, SoyAdi) VALUES ('Alperen','Bektaşoğlu')

 -- Örnek 6 (Personeller tablosuna bir kayıt güncellendiği zaman; değişen kaydın adı, eklenen kaydın adı, kim tarafından ve hangi tarihte eklendiğini başka bir tabloya kayıt edilsin.) (Log Tablosu Örneği - 2)
CREATE TRIGGER Log_Personel_Guncelleme_Trigger
ON Personeller
AFTER UPDATE
AS
DECLARE @EskiAd NVARCHAR(MAX), @YeniAd NVARCHAR(MAX)
SELECT @EskiAd = Adi FROM deleted
SELECT @YeniAd = Adi FROM inserted
INSERT LogTablosu VALUES ('Eski Adı ' + @EskiAd + ' olan personel, yeni adı ' + @YeniAd + ' olarak, ' + SUSER_NAME() + ' tarafından ' + CAST(GETDATE() AS nvarchar(MAX)) + ' tarihinde güncellenmiştir.')

UPDATE Personeller SET Adi = 'Gökçen' WHERE SoyAdi = 'Bektaşoğlu'

-- Örnek 7 (Personeller tablosuna bir kayıt silindiği zaman; silinen kaydın adı, kim tarafından ve hangi tarihte silindiği başka bir tabloya kayıt edilsin.) (Log Tablosu Örneği - 3)
CREATE TRIGGER Log_Personel_Silme_Trigger
ON Personeller
AFTER DELETE
AS
DECLARE @Ad NVARCHAR(MAX)
SELECT @Ad = Adi FROM deleted
INSERT LogTablosu VALUES ('Adı ' + @Ad + ' olan personel, '  + SUSER_NAME() + ' tarafından ' + CAST(GETDATE() AS nvarchar(MAX)) + ' tarihinde silinmiştir.')

DELETE Personeller WHERE SoyAdi = 'Bektaşoğlu'

 -- Örnek 8 (Personeller tablosunda adı "Andrew" olan kaydın silinmesini engelleyen ama diğerlerine izin veren triggerı yazalım.)
CREATE TRIGGER Admin_Koruma_Trigger
ON Personeller
AFTER DELETE
AS
DECLARE @Ad NVARCHAR(MAX)
SELECT @Ad = Adi FROM deleted
IF @Ad = 'Andrew'
	BEGIN
	PRINT 'Bu kayıt silinemez.'
	ROLLBACK -- Yapılan tüm işlemleri geri alır. Transaction konusunda detaylı göreceğiz.
	END
ELSE
	BEGIN
	PRINT 'Kayıt silme işlemi başarılı'
	END

INSERT Personeller(Adi, SoyAdi) VALUES ('Alperen','Bektaşoğlu')
INSERT Personeller(Adi, SoyAdi) VALUES ('Andrew','Totti')

DELETE Personeller WHERE SoyAdi = 'Bektaşoğlu'
DELETE Personeller WHERE SoyAdi = 'Totti'

-- Örnek 9 (INSTEAD OF Trigger Örneği) (Personeller tablosuna update işlemi gerçekleştiği anda yapılacak güncelleştirme yerine bir log tablosunda güncellenecek kaydın adı, yeni kaydın adı, kim tarafından ve hangi tarihte güncellenmeye çalışıldığı başka bir tabloya kayıt edilsin.)
CREATE TRIGGER Log_Personel_Guncelleme_Engelleme_Trigger
ON Personeller
INSTEAD OF UPDATE
AS
DECLARE @EskiAd NVARCHAR(MAX), @YeniAd NVARCHAR(MAX)
SELECT @EskiAd = Adi FROM deleted
SELECT @YeniAd = Adi FROM inserted
INSERT LogTablosu VALUES ('Eski Adı ' + @EskiAd + ' olan personel, yeni adı ' + @YeniAd + ' olarak, ' + SUSER_NAME() + ' tarafından ' + CAST(GETDATE() AS nvarchar(MAX)) + ' tarihinde güncellenmek istendi.')

INSERT Personeller(Adi, SoyAdi) VALUES ('Alperen','Bektaşoğlu')
UPDATE Personeller SET Adi = 'Gökçen' WHERE SoyAdi = 'Bektaşoğlu'
DELETE Personeller WHERE SoyAdi = 'Bektaşoğlu'
```

## Trigger'ı Devre Dışı Bırakma - Aktifleştirme

```sql
-- Trigger'ı Devre Dışı Bırakma
DISABLE TRIGGER OrnekTrıgger_1 ON Personeller
DISABLE TRIGGER Tablo_Silme_Engelleme_Trigger_1 ON DATABASE

-- Trigger'ı Aktifleştirme
ENABLE TRIGGER OrnekTrıgger_1 ON Personeller
ENABLE TRIGGER Tablo_Silme_Engelleme_Trigger_1 ON DATABASE
```

## Alter Komutu İle DDL Trigger Güncelleme 

```sql
CREATE TABLE LogTablosu(
Id INT PRIMARY KEY IDENTITY(1,1),
Rapor NVARCHAR(MAX)
)

-- Örnek
ALTER TRIGGER Tablo_Silme_Engelleme_Trigger_1
ON DATABASE
AFTER drop_table
AS
PRINT 'Bu işlem Tablo_Silme_Engelleme_Trigger Triggeri tarafından engellenmiştir. Yeni Trigger'
ROLLBACK

DROP TABLE LogTablosu
```

## Alter Komutu İle DML Trigger Güncelleme

```sql
-- Kullanım Protitipi:

-- ALTER TRIGGER [ Trigger Adı ]
-- ON [ Tablo Adı ]
-- AFTER/FOR/INSTEAD OF INSERT, UPDATE, DELETE
-- AS
-- [Çalışması İstenen Kodlar] 
```

## Trigger Kaldırma

```sql
DROP TRIGGER Tablo_Silme_Engelleme_Trigger_1
```
