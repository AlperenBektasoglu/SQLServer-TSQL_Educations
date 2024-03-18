# Özel Fonksiyonlar

## Aggregate Fonksiyonları

```sql
USE Northwind

-- AVG : Ortalama alır. (average)
SELECT AVG(PersonelID) FROM Personeller

-- MAX : En büyük değeri bulur.
SELECT MAX(PersonelID) FROM Personeller

-- MIN : En küçük değeri bulur.
SELECT  MIN(PersonelID) FROM Personeller

-- COUNT : Toplam satır sayısını verir.
SELECT COUNT(*) FROM Personeller     -- Personeller tablosundaki tüm satırların sayısını döndürecektir.
SELECT COUNT(Adi) FROM Personeller   -- Personeller tablosundaki 'Adi' sütununda null olmayan değerlerin sayısını döndürecektir.

-- SUM : Bir sütundaki değerlerin toplamını verir.
SELECT SUM(NakliyeUcreti) FROM Satislar
```

## String Fonksiyonları

```sql
-- LEFT : Soldan(baştan) belirtilen sayıda karakteri getirir.
SELECT Adi , LEFT(Adi, 2) FROM Personeller

-- RIGHT : Sağdan(sondan) belirtilen sayıda karakteri getirir.
SELECT Adi , RIGHT(Adi, 3) FROM Personeller

-- UPPER : Büyük harfe çevirir.
SELECT UPPER(Adi) FROM personeller

-- LOWER : Küçük harfe çevirir.
SELECT LOWER(Adi) FROM personeller

-- SUBSTRING : Belirtilen sıra numarasından itibaren belirtilen sayıda karakter getirir. Sıra numarası 1 den başlar.
SELECT 'Alperen', SUBSTRING('Alperen', 2, 5) -- Sonuç: 'lpere'
SELECT Soyadi, SUBSTRING(SoyAdi,2,5) FROM Personeller -- 2. karakterden itibaren 5 karakteri getirir

-- LTRIM : Soldan boşlukları siler.
SELECT '          Alperen'
SELECT LTRIM('          Alperen')

-- RTRIM : Sağdan boşlukları siler.
SELECT 'Bektaşoğlu          '
SELECT RTRIM('Bektaşoğlu          ')

-- REVERSE : Sütundaki veriyi tersten yazar.
SELECT	Adi, REVERSE(Adi) FROM Personeller

-- REPLACE : Belirtilen ifadeyi belirtilen ifade ile değiştirir.
SELECT 'İstanbul dünyanın en iyi şehiridir!!!', REPLACE('İstanbul dünyanın en iyi şehiridir!!!', 'İstanbul', 'İzmir')

-- CHARINDEX : Belirtilen karakterin metin içindeki sıra numarasını verir.
SELECT 'Alperen', CHARINDEX('l', 'Alperen') -- Alperen | 2
SELECT Adi , CHARINDEX('a', Adi) FROM Personeller
```

Karmaşık bir örnek üzerinde öğrendiklerimizi kullanalım:

```sql
-- Müşteriler tablosunda MusteriAdi kolonundan sadece soyadlarını çekelim.
SELECT MusteriAdi , SUBSTRING(MusteriAdi, CHARINDEX(' ',MusteriAdi), LEN(MusteriAdi)) FROM Musteriler
SELECT MusteriAdi , SUBSTRING(MusteriAdi, CHARINDEX(' ',MusteriAdi), LEN(MusteriAdi)- CHARINDEX(' ',MusteriAdi) +1) FROM Musteriler

-- LEN(MusteriAdi)  Maria Anders --> 12
-- CHARINDEX(' ',MusteriAdi) Boşluk karakteri konumu --> 6
-- LEN(MusteriAdi)- CHARINDEX(' ',MusteriAdi) +1
-- 12-6+1 = 7
```

## Sayısal Değer İşlemleri ve Sayısal Fonksiyonlar

```sql
SELECT 2+5
SELECT 3-6
SELECT 3*4
SELECT 12/6

-- PI = Pi sayısını verir.
SELECT PI()

-- SIN = Sinüs alır.
SELECT SIN(90)

-- COS = Cosinüs alır.
SELECT COS(90)

-- TAN = Tanjant alır.
SELECT TAN(90)

-- COT = Cotanjant alır.
SELECT COT(90)

-- POWER : Üs alır.
SELECT POWER(2,3)

-- SQRT : Karekök alır.
SELECT SQRT(4) as DordunKarekökü

-- ABS : Mutlak değer alır. (Absolute Value)
SELECT ABS(-3)

-- RAND : 0-1 arasında rastgele bir sayı üretir. (Random)
SELECT RAND()

-- FLOOR : Küsüratı atar.
SELECT FLOOR(3.6) -- Çıktı: 3

-- ROUND : Yuvarlama yapar.
SELECT ROUND(3.4) -- Çıktı: 3
SELECT ROUND(3.5) -- Çıktı: 4
SELECT ROUND(3.6) -- Çıktı: 4
```

## Tarih Fonksiyonları

```sql
-- GETDATE : Bugünün tarihini verir.
SELECT GETDATE()

-- DATEADD : Verilen tarihe verildiği kadar gün , ay ve yıl ekler.
SELECT DATEADD(DAY, 100, GETDATE())
SELECT DATEADD(MONTH, 100, GETDATE())
SELECT DATEADD(YEAR, 100, '07.05.2000') -- Syntax = ay/gün/yıl  Viev = yıl/ay/gün

-- DATEDIFF : İki tarih arasında günü , ayı veya yılı hesaplar.
SELECT DATEDIFF(DAY,'07.05.2018',GETDATE())
SELECT DATEDIFF(MONTH,'07.05.2018',GETDATE()) -- Çıktı: 68
SELECT DATEDIFF(MONTH,GETDATE(),'07.05.2018') -- Çıktı: -68
SELECT DATEDIFF(YEAR,'07.05.2018',GETDATE())

-- DATEPART : Verilen tarihin haftanın ,ayın yahut yılın kaçıncı günü olduğunu hesaplar.
SELECT
GETDATE(),
DATEPART(YEAR, GETDATE()) AS Yıl,
DATEPART(MONTH, GETDATE()) AS Ay_1,
DATEPART(DW,GETDATE()) AS Ay_2,
DATEPART(DAY, GETDATE()) AS Gun,
DATEPART(HOUR, GETDATE()) AS Saat,
DATEPART(MINUTE, GETDATE()) AS Dakika,
DATEPART(WEEK, GETDATE()) AS Hafta_Numarasi,
DATEPART(QUARTER, GETDATE()) AS Çeyrek

-- DATENAME : Verilen tarihte haftanın ,ayın yahut yılın ismini verir.
SELECT
GETDATE(),
DATENAME(MONTH, GETDATE()) as Ay,
DATENAME(WEEK, GETDATE()) as Hafta,
DATENAME(WEEKDAY, GETDATE()) as Gun,
DATENAME(MINUTE, GETDATE()) as Dakika
```

## Coalesce Fonksiyonu İle Null Değer Kontrolü

Birinci parametre olarak verilen kolondaki null değerleri, ikinci parametrede ki değer ile değiştirir.

```sql
SELECT MusteriAdi, COALESCE(Bolge, 'Bölge bilinmiyor') FROM Musteriler
```

## IsNull Fonksiyonu İle Null Değer Kontrolü

Birinci parametre olarak verilen kolondaki null değerleri, ikinci parametrede ki değer ile değiştirir.

```sql
SELECT MusteriAdi, ISNULL(Bolge, 'Bölge bilinmiyor') FROM Musteriler
```

##  NullIf Fonksiyonu İle Null Değer Kontrolü

Fonksiyona verilen birinci parametre, ikinci parametredeki değere eşit ise birinci parametreyi null olarak getirir. Değil ise birinci parametreyi olduğu gibi getirir.

```sql
SELECT NULLIF(0, 0) -- Çıktı: NULL
SELECT NULLIF(5, 0) -- Çıktı: 5
```

## ROW_NUMBER() Fonksiyonu

Temelde işlevi her bir satıra karşılık primary kolonundan bağımsız olarak sıralı bir index numarası atanmış kolon tanımlamaktır. Ama bilmemiz gereken ROW_NUMBER() fonksiyonu OVER() fonksiyonu ile birlikte kullanılmaktadır.

```sql
SELECT ROW_NUMBER() OVER(ORDER BY Adi) Indexer, * FROM Personeller
SELECT ROW_NUMBER() OVER(ORDER BY Adi) Indexer, * FROM Personeller ORDER BY PersonelId -- Bu örnekte verilen Indexer değerlerinin değişmediğini göreceksin.
```

<a href="https://www.gencayyildiz.com/blog/transact-sql-row_number-fonksiyonu/"> Kaynak </a>
