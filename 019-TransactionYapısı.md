# Transaction Yapısı

Belirli bir grup işlemin arka arkaya gerçekleşmesine rağmen, işlemlerin seri ya da toplu halde değerlendirilip hepsinin düzgün bir şekilde ele alınması gerektiğinde kullanılır. Transaction içinde yer alan tüm işlemler sorunsuz bir şekilde çalışmak zorunda, aksi halde transaction içindeki işlemlerin tek bi adımı dahi başarısız olsa, tüm yapılan işlemler yapılmamış gibi eski haline döner.

BEGIN TRANSACTION / BEGIN TRAN : Transaction işlemini başlatır.

ROLLBACK / ROLLBACK TRANSACTION / ROLLBACK TRAN : İşlemler geri alır ve iptal eder.

COMMIT / COMMIT TRANSACTION / COMMIT TRAN : İşlemleri kaydeder.

**Not:** Rollback ve commit işlemi gerçekleştiği zaman transaction işlemi sonlanır.

**Not:** Biz transaction yapısını kullanmasak bile normalde veritabanında yapılan her işlem(Insert, Update, Delete, ...) BEGIN TRAN ile başlayıp, COMMIT TRAN ile biter.

```sql
-- Kullanım Prototipi:
BEGIN TRANSACTION [ Transaction Adı ] -- Transaction'a isim vermek zorunlu değildir.
[işlemler]
```

```sql
USE Northwind

-- Örnek 1
BEGIN TRANSACTION
SELECT * FROM Personeller WHERE Unvan = 'Sales Manager'
UPDATE Personeller SET Unvan = 'Information System' WHERE Unvan = 'Sales Manager'
ROLLBACK

-- Örnek 2
BEGIN TRANSACTION
SELECT * FROM Personeller WHERE Unvan = 'Sales Manager'
UPDATE Personeller SET Unvan = 'Information System' WHERE Unvan = 'Sales Manager'
ROLLBACK
SELECT * FROM Personeller WHERE Unvan = 'Sales Manager'
UPDATE Personeller SET Unvan = 'Information System' WHERE Unvan = 'Sales Manager'
ROLLBACK
SELECT * FROM Personeller WHERE Unvan = 'Sales Manager'

-- Örnek 3
BEGIN TRANSACTION
UPDATE Personeller SET Unvan = 'Database Manager' WHERE Unvan = 'Sales Representative'
UPDATE Personeller SET UnvanEki = 'Mrs.' WHERE Adi = 'Steven'
SELECT * FROM Personeller
ROLLBACK
SELECT * FROM Personeller

-- Örnek 4
BEGIN TRAN Kontrol
DECLARE @i INT
DELETE FROM Personeller WHERE PersonelID > 9
SET @i = @@ROWCOUNT -- @@ROWCOUNT: Etkilenen satır sayısını getirir.
IF @i = 1
	BEGIN
	PRINT 'Kayıt silindi.'
	COMMIT
	END
ELSE
	BEGIN
	PRINT 'İşlemler geri alındı.'
	ROLLBACK TRAN
	END
```

## WAITFOR Kullanımı

Sorguyu istenilen süreden sonra yada istenilen saatte çalıştıran komuttur.

```sql
-- Örnek 1
WAITFOR DELAY '00:00:05' -- 'saat : dakika : saniye'
SELECT * FROM Urunler

-- Örnek 2
WAITFOR TIME '15:04:30' -- 'saat : dakika : saniye'
SELECT * FROM Personeller
```
