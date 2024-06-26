# Diğer Konular

## IDENTITY Kullanımı

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

-- Çıktı:
-- 6
-- 1
-- 1
```

## ROWCOUNT Kullanımı

DML (Data Manipulation Language – select, insert, update, delete) işleminden etkilenen satırların toplam sayısını döndürür.

```sql
USE Northwind
SELECT * FROM Personeller
SELECT @@ROWCOUNT
```

## Veritabanındaki Tabloları Listeleme

```sql
SELECT * FROM sys.tables
```

## Bir Tabloda Primary Key Olup Olmadığını Kontrol Etme

```sql
SELECT OBJECTPROPERTY(OBJECT_ID('Personeller'), 'TableHasPrimaryKey')
```

## En Son Primary Key Id Değerinin Bulunması

```sql
SELECT IDENT_CURRENT('Personeller')
```

## Default Values Kullanımı

Bu komut ile tabloya sadece primary key alanı dolacak şekilde kayıt ekleyebiliriz. Diğer kolonlara null veya default değerler atanacaktır.

```sql
USE Nortwind
INSERT Personeller DEFAULT VALUES
```

## ROW_NUMBER() Fonksiyonu

Temelde işlevi her bir satıra karşılık primary kolonundan bağımsız olarak sıralı bir index numarası atanmış kolon tanımlamaktır. Ama bilmemiz gereken ROW_NUMBER() fonksiyonu OVER() fonksiyonu ile birlikte kullanılmaktadır.

```sql
USE Nortwind
SELECT ROW_NUMBER() OVER(ORDER BY Adi) Indexer, * FROM Personeller
SELECT ROW_NUMBER() OVER(ORDER BY Adi) Indexer, * FROM Personeller ORDER BY PersonelId -- Bu örnekte verilen Indexer değerlerinin değişmediğini göreceksin.
```

Şimdi de bir tabloda tekrar eden datalar içerisinde gruplama işlemi yaparak indexlemenin prototipini inceleyelim.

```sql
USE Nortwind
SELECT ROW_NUMBER() OVER(PARTITION BY MusteriId ORDER BY OdemeTarihi) Indexer, * FROM Satislar

-- Çıktı:
-- Indexer - MusteriId
-- 1	ALFKI
-- 2	ALFKI
-- 3	ALFKI
-- 1	ANATR
-- 2	ANATR
```

## Ansi_Nulls Komutu

Ansi_Nulls komutu, where şartlarında kontrol edilen eşitlik yahut eşit değillik durumlarında Null değerlerin dikkate alınıp alınmayacağını belirlememizi sağlayan bir özelliktir. Prototipi aşağıdaki gibidir:

```sql
SET ANSI_NULLS [ ON | OFF ]
```

Ansi_Nulls özelliği "On" değerini aldığı vakit, eşitlik yahut eşit değillik şartlarında null değerlere karşı false sonucu döndürülür. Bunun sebebi sorgu aramalarına null değerlerin dahil edilmemesinden kaynaklanmaktadır. Bilakis "Off" değeri aldığı vakit null değerler sorgu sonucuna dahil edileceğinden dolayı şartın durumuna göre gerekli true/false değerini dönecektir. Ansi_Nulls özelliği varsayılan olarak "On" değerine sahiptir.

## Dynamic Data Masking

Adından da anlaşılacağı gibi dinamik veri maskeleme sistemidir. Veritabanımızda, ilişkisel tablolarımızda tuttuğumuz verileri tararken gösterilmesi istenen bilgiler dışındakileri maskeleme özelliğidir. Tabi ki de burada tablodaki bilgiden kastımız kolonlardır. Bir tablo create yahut alter işlemine tabi tutulurken hangi kolonun gizleneceğini artık Dynamic Data Masking özelliğiyle işaretleyebiliyoruz.

Detaylı incelemek için <a href="https://www.gencayyildiz.com/blog/sql-server-2016-dynamic-data-masking/"> tıklayın </a>

## Temporal Tables

Bu özelliğin özeti, veritabanında yapılan Data Manipulation Language(DML) işlemlerini raporlamamızı sağlayan bir yapıdır. Şuana kadar yaptığımız raporlama ve takip sistemlerinde izlediğimiz adımlar oluşturduğumuz tablolara özel “log” tabloları yahut dosyaları oluşturmaktan ibaretti. Yapılan her işlemi ya log tablolarına sorgular eşliğinde kayıt eder yahut trigger‘lar aracılığıyla takip sürecini gerçekleştirirdik. Temporal Tables özelliği ile bu süreci hem teknik olarak daha kullanışlı, hemde pratikte daha az maliyetli bir şekilde gerçekleştiriyor olacağız.

Temporal Tables özelliğinin bize sağlayacağı artıları kısaca maddelersek eğer;

- Tablomuzda bulunan kayıtların zaman içindeki değişikliklerinin(update) izlenmesi ve takip edilmesi,
- Tablo üzerinde yanlışlıkla yapılan delete ve update sorgularının geri getirilmesi,
- Bir verinin belirli bir zamana yahut zaman aralığına odaklı izlenebilmesi.

Detaylı incelemek için <a href="https://www.gencayyildiz.com/blog/sql-server-2016-temporal-tables/"> tıklayın </a>

## Row Level Security

SQL Server’da hangi kullanıcının hangi tablo üzerinde hangi işlemi yapabileceğini belirleyebiliyorduk. Lakin kullanıcıların kendilerini ilgilendiren verileri görmesini belirleyemiyorduk. Ta ki, SQL Server 2016 ile gelen Row Level Security özelliği gelene kadar…

Detaylı incelemek için <a href="https://www.gencayyildiz.com/blog/sql-server-2016-row-level-security/"> tıklayın </a>

## Execution Plan Nedir?

Yapılan sorguların çoğunluğunu veri okuma işleminin oluşturduğunu düşündüğümüzde, son kullanıcıya istenilen verilerin en kısa sürede iletilmesi oldukça önemlidir. İşte bu noktada SQL Server gelen sorguyu derler ve hangi tabloya gitmesi gerektiği, bu tabloya giderken (varsa) hangi indexi kullanacağı, hangi verileri alacağı ve bunları yaparken istatistiklerden yararlanmak gibi birçok detayı değerlendirir ve plan çıkarır. Arka planda yapmış olduğu bu değerlendirmeler sonrasında, sorgu sonucunu getirirken en az maliyetli planın hangisi olduğuna karar verir. Sorguyu yürütürken kullanacağı bu plana Execution Plan denir. Execution Plan bir kere belirlendikten sonra cache’e alınır ve bir sorgu geldiğinde cachede tutulan planları tarayarak aynı ya da benzer sorgu mevcutsa o execution plan üzerinden sonuçları döndürür.

**Not:** Execution Plan okurken sırası ile yukarıdan aşağıya ve sağdan sola doğru takip edilir. Operatörler arası geçiş sırasında zaten oklar yer almaktadır. Bunları takip etmek amacıyla kullanabileceğiniz gibi diğer operatöre geçiş yaparken aktarılan verinin büyüklüğüyle ilgili bilgi sahibi olmak için de kullanabilirsiniz. Operatörün üzerine geldiğinizde beliren tooltip kısmından da bu verilerin boyutlarını görebilirsiniz.

Detaylı incelemek için <a href="https://www.sqlekibi.com/sql-server/sql-server-execution-plan-nedir.html/"> tıklayın </a>

## Sql Bulk Insert

Verileri bir metin dosyasından, veritabanında ki bir tabloya toplu ekleme işlemine bulk insert denir.

Detaylı incelemek için <a href="https://www.youtube.com/watch?v=7nIppLEf5bQ&list=PLQVXoXFVVtp2RjHt5teaBOLUcKbq2Ilbo&index=39"> tıklayın </a>

## DCL (Data Control Language - Veri Kontrol Dili)

DCL, bir veritabanı ile ilişkili kullanıcıları ve rollerin izinlerini değiştirmek için yani verilere erişim yetkilerini düzenlemek amacıyla kullanılır.

Detaylı incelemek için <a href="https://ibrahimbayraktar.net/sqlde-yetkilendirme-grant-deny-revoke.html"> tıklayın </a>
