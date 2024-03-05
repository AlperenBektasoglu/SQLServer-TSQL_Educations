# Fonksiyonlar

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















