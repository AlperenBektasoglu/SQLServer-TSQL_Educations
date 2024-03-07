
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
CREATE PROC OrnekProcedure_4 (@sayi1 INT, @sayi2 INT, @sayi3 INT, @sayi4 INT, @sonuc1 INT OUT, @sonuc2 INT OUT)
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

## Alter Komutu İle Stored Procedure Yapısını Değiştirme

```sql
-- Örnek 1
ALTER PROC OrnekProcedure_3 (@sayi1 INT , @sayi2 INT , @sonuc INT OUTPUT)
AS
SET @sonuc = @sayi1 + @sayi2 + 20
PRINT 'Sonuç: ' + CAST(@sonuc AS VARCHAR(9))

DECLARE @x INT
EXEC OrnekProcedure_3 33, 5, @x OUTPUT
PRINT '@x değişkenindeki değer: ' + CAST(@x AS VARCHAR(9))

-- Örnek 2
ALTER PROC OrnekProcedure_3 (@sayi1 INT , @sayi2 INT , @sayi3 INT, @sonuc INT OUTPUT)
AS
SET @sonuc = @sayi1 + @sayi2 + @sayi3
PRINT 'Sonuç: ' + CAST(@sonuc AS VARCHAR(9))

DECLARE @x INT
EXEC OrnekProcedure_Topla 33, 5, 50, @x OUTPUT
PRINT '@x değişkenindeki değer: ' + CAST(@x AS VARCHAR(9))
```

## Stored Procedure İle With Encryption Komutunun Kullanımı

With encryption komutu kullanılarak yazılmış prosedürün içeriği şifrelenerek saklanır, kodu gösterilmez. Bununla birlikte Şifrelenmiş bir Stored Procedure, alter komutu ile güncellenirken her seferinde with encryption ifadesi kullanılmalıdır. Kullanılmazsa şifrelemeden vazgeçildiği şeklinde düşünülerek çalıştırılır.

```sql
-- Örnek 1
CREATE PROCEDURE OrnekProcedure_UlkeyeGöreGetir (@ulke VARCHAR(15))
WITH ENCRYPTION
AS
SELECT * FROM Personeller WHERE Ulke = @ulke

EXEC OrnekProcedure_UlkeyeGöreGetir 'UK'
```

## Stored Procedure İle With Recompile Komutunun Kullanımı

Stored Procedure(SP) ilk çalıştırıldığı zaman istatistikler göz önüne alınarak Query Optimizer tarafından en optimum sorgu planı(Query Plan) oluşturulur ve daha sonra kullanılmak üzere plan cache’e konulur. Aynı SP farklı bir zamanda tekrar çalıştırıldığında cache’deki plan’ın geçerliliği kontrol edilir ve eğer plan geçerli yani güncel ise tekrar query plan oluşturulmak için zaman harcanmayıp, plan cache’den çağırılır ve kullanılır. Query plan oluşturma işlemi bazı durumlarda çok fazla CPU kaynağı tükettiği için bu şekilde bir cache’lenme mekanizması kullanılır. Fakat bazı durumlarda cache’lenen plan güncelliğini yitirmiş olabilir. Örneğin SP içinde geçen bir tabloda, plan cache’lendikten sonra çok fazla data değişimi olduysa bu durumda cache’lenen plan güncelliğini yitirecektir. Ya da SP’nin içinde geçen tablolarda index ekleme, silme gibi DDL (Data Definition Language) değişiklikleri yapılırsa yine cache’lenen plan güncelliğini yitirmiş olacaktır. Böyle bir durumda SP’nin yeniden derlenip yeni bir query plan’ın oluşturulması gerekmektedir. İşte bu duruma Recompilation denilmektedir.

```sql
-- Örnek 1
CREATE PROCEDURE OrnekProcedure_PersonelGetir
WITH RECOMPILE -- Her çağrılmada yeniden düzenleniyor. (Compile ediliyor.)
AS
SELECT * FROM Personeller

-- Örnek 2 ( With Recompile Komutunu Kaldırma)
ALTER PROCEDURE OrnekProcedure_PersonelGetir
AS
SELECT * FROM Personeller

-- Örnek 3
ALTER PROCEDURE OrnekProcedure_PersonelGetir
WITH RECOMPILE -- Her çağrılmada yeniden düzenleniyor.(compile ediliyor)
AS
SELECT * FROM Personeller WHERE Sehir = 'London'
```

## Stored Procedure İle Set Nocount On Komutunun Kullanımı

Sql Server‘da her sorgu çalıştırdığımızda; sorgu sonucu, etkilenen satır sayısı ile birlikte sorguyu çalıştıran uygulamaya geri gönderilir. Bazı durumlarda bu bilgi işimize yarasa bile genellikle kullanmayız. Sql Server‘ın bu bilgiyi hesaplamasını ve uygulamaya geri göndermesini engelleyerek, çok ufakta olsa performastan kazanç sağlayabiliriz. Özetle bu komut ile Sql Server sadece ilgili sorgu için, etkilenen satır sayısını hesaplama işlemini yapmayacaktır. Yapmamız gereken, sorgudan önce aşağıdaki komutu çalıştırmak olacaktır;

SET NOCOUNT ON 

```sql
-- Örnek 1
INSERT Personeller(Adi, SoyAdi) VALUES('Alperen','Bektaşoğlu')

SET NOCOUNT ON
INSERT Personeller(Adi, SoyAdi) VALUES('Gökçen','Bektaşoğlu')

SET NOCOUNT OFF
INSERT Personeller(Adi, SoyAdi) VALUES('Gökçen','Bektaşoğlu')

DELETE FROM Personeller WHERE Adi = 'Alperen' OR Adi = 'Gökçen'

-- Örnek 2
CREATE PROCEDURE OrnekProcedure_PersonelGetir_1
AS
SELECT * FROM Personeller

EXEC OrnekProcedure_PersonelGetir_1

CREATE PROCEDURE OrnekProcedure_PersonelGetir_2
AS
SET NOCOUNT ON
SELECT * FROM Personeller

EXEC OrnekProcedure_PersonelGetir_2

-- Örnek 3
ALTER PROCEDURE OrnekProcedure_PersonelGetir_1
AS
SET NOCOUNT ON
SELECT * FROM Personeller

EXEC OrnekProcedure_PersonelGetir_1
```

## Stored Procedure Silme

```sql
DROP PROC OrnekProcedure_1
DROP PROCEDURE OrnekProcedure_1
```

