
# Stored Procedures (SP)

Veritabanı programlamada belirli işlemleri yerine getirmek için yazılıp derlenmiş ve gerektiğinde defalarca çağrılabilen kod bloklarıdır. Her ihtiyaç duyulduğunda çağrılabilirler. Ayrıca bir sp, başka bir sp tarafından da çağrılabilir.

Stored Procedure Çeşitleri:
1. System Stored Procedures: Genellikle «sp_» öneki başlayan ve master veritabanı içerisinde tutulan kod bloklarıdır.
2. User Defined Stored Procedures: Kullanıcılar tarafından yazılmış olan kod bloklarıdır.
3. Store Procedure'ler parametre alıp, çıktı üretebilirler.

**Not:** Bir sp, CREATE DEFAULT, CREATE PROC, CREATE RULE, CREATE TRIGGER VE CREATE VIEW gibi kodları içeremez. Ancak view, fonksiyon, procedure, tablo gibi veritabanı sisteminde ki nesnelerden veri alabilir yada aktarabilir. 

## Stored Procedure Oluşturma

```sql
USE Northwind

-- Örnek 1
CREATE PROCEDURE OrnekProcedure_1
AS
SELECT * FROM Personeller WHERE BagliCalistigiKisi < 5

EXEC OrnekProcedure_1
EXECUTE OrnekProcedure_1

-- Örnek 2 (Parametreli ve Çıktılı Stored Procedure Örneği)
CREATE PROC OrnekProcedure_2 @sayi1 INT , @sayi2 INT , @sonuc INT OUTPUT
AS
SET @sonuc = @sayi1 + @sayi2

DECLARE @x INT
EXEC OrnekProcedure_2 33, 5, @x OUTPUT
PRINT @x

DECLARE @x INT
EXEC OrnekProcedure_Topla 33, 5, @x OUTPUT
PRINT @x

-- Örnek 3
CREATE PROC OrnekProcedure_3 (@sayi1 INT, @sayi2 INT, @sayi3 INT, @sonuc INT OUT)
AS
SET @sonuc = (@sayi1 + @sayi2 + @sayi3)/3
PRINT 'Sonuç: ' + CAST(@sonuc AS VARCHAR(9))

DECLARE @Ort INT
EXEC OrnekProcedure_3 10, 20, 30, @Ort OUT
PRINT '@Ort değişkenindeki değer: ' + CAST(@Ort AS VARCHAR(9))

-- Örnek 4
CREATE PROC OrnekProcedure_4 (@sayi1 INT , @sayi2 INT , @sayi3 INT, @sayi4 INT , @sonuc1 INT OUT, @sonuc2 INT OUT)
AS
SET @sonuc1 = (@sayi1 + @sayi2)
SET @sonuc2 = (@sayi3 + @sayi4)
PRINT '1. Sonuç: ' + CAST(@sonuc1 AS VARCHAR(9))
PRINT '2. Sonuç: ' + CAST(@sonuc2 AS VARCHAR(9))

DECLARE @s1 INT, @s2 INT
EXEC OrnekProcedure_4 10, 20, 30, 40, @s1 OUT, @s2 OUT
PRINT '@s1 değişkenindeki değer: ' + CAST(@s1 AS VARCHAR(9))
PRINT '@s2 değişkenindeki değer: ' + CAST(@s2 AS VARCHAR(9))

-- Örnek 5
CREATE PROCEDURE OrnekProcedure_SehireGöreGetir_1 (@sehir VARCHAR(15))
AS
SELECT * FROM Personeller WHERE Sehir = @sehir

EXECUTE OrnekProcedure_SehireGöreGetir_1 'London'

-- Örnek 6
CREATE PROCEDURE OrnekProcedure_SehireGöreGetir_2 (@sehir VARCHAR(15) = 'Seattle')
AS
SELECT * FROM Personeller WHERE Sehir = @sehir

EXECUTE OrnekProcedure_SehireGöreGetir_2
EXECUTE OrnekProcedure_SehireGöreGetir_2 'London'
```
