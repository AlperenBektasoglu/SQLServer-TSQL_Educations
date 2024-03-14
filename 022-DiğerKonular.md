# Diğer Konular

## Sql Bulk Insert
<a href="https://www.youtube.com/watch?v=7nIppLEf5bQ&list=PLQVXoXFVVtp2RjHt5teaBOLUcKbq2Ilbo&index=39"> Video kaynağı için tıklayın. </a>

## @@IDENTITY Kullanımı

T-SQL’de identity kolonlarla çalışırken yeni üretilen değerleri elde etmemiz gerekebilir. İşte bu ihtiyaca dönük @@IDENTITY komutu yahut SCOPE_IDENTITY() ve IDENT_CURRENT() fonksiyonları kullanılabilir.
1. @@IDENTITY: Açılmış olan bağlantıda(connection) tablo yahut sorgunun çalıştığı scope’a bakmaksızın son üretilen identity değerini vermektedir. Dikkat! Trigger kullanılan sorgularda yanlış sonuç alma ihtimalinden dolayı kullanılması tavsiye edilmez.
2. SCOPE_IDENTITY: Açılmış olan bağlantıda(connection) ve sorgunun çalıştığı scope’ta son üretilen identity değerini döndürür. Dikkat! Trigger kullanılan sorgularda @@IDENTITY yerine bu fonksiyonun kullanılması tavsiye edilir.
3. IDENT_CURRENT(TabloAdi): Bağlantı ve sorgunun çalıştırıldığı scope’a bakmaksızın parametre olarak verilen tabloda üretilen sonuncu identity değerini döndürür.

```sql
CREATE DATABASE ORNEKVERITABANI
 
CREATE TABLE ORNEKTABLO1
(
  ID INT PRIMARY KEY IDENTITY,
  KOLON1 NVARCHAR(MAX),
  KOLON2 NVARCHAR(MAX)
)
 
CREATE TABLE ORNEKTABLO2
(
  ID INT PRIMARY KEY IDENTITY,
  KOLON1 NVARCHAR(MAX),
  KOLON2 NVARCHAR(MAX)
)

USE ORNEKVERITABANI
 
CREATE TRIGGER KONTROL
ON ORNEKTABLO1 FOR INSERT
AS
INSERT ORNEKTABLO2 VALUES('', '')

INSERT ORNEKTABLO2 VALUES('1','1')
INSERT ORNEKTABLO2 VALUES('2','2')
INSERT ORNEKTABLO2 VALUES('3','3')
INSERT ORNEKTABLO2 VALUES('4','4')
INSERT ORNEKTABLO2 VALUES('5','5')

INSERT ORNEKTABLO1 VALUES('6','6')

SELECT @@IDENTITY
UNION ALL
SELECT SCOPE_IDENTITY()
UNION ALL
SELECT IDENT_CURRENT('ORNEKTABLO1')
```

