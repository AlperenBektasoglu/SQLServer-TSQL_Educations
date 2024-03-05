
# Temel Sql Komutları

## Select Komutu

Seç ve getir işlemi yapar.

```sql
USE Northwind

SELECT 3    
SELECT 'Alperen'
PRINT 'Alperen'
SELECT 'Alperen' , 'Bektaşoğlu' , 21 , 42
SELECT * FROM Personeller
SELECT Adi , SoyAdi FROM Personeller
```

## Alias(Takma Ad) Kulanımı

Sütuna isim atama işlemi yapar.'as' ifadesinin kullanılması zorunlu değildir.

```sql
SELECT 'Alperen' as Ad , 'Bektaşoğlu' Soyad
SELECT Adi isim , SoyAdi Soyisim FROM Personeller

-- Boþluk karakteri olunca Alias atama
SELECT 'Alperen Bektaþoðlu' [Ad Soyad]

-- Boşluk karakteri içeren tabloyu sorgulama
SELECT * FROM [Satis Detaylari]
```

## Kolon Birleştirme

```sql
-- Aynı tipteki kolonları birleştirme
SELECT Adi + ' ' + SoyAdi  AdSoyad FROM Personeller

-- Farklı tipteki kolonları birleştirme
SELECT Adi + ' ' + IseBaslamaTarihi  FROM Personeller --Hatalı işlem
SELECT Adi + ' ' + CONVERT(nvarchar , IseBaslamaTarihi) FROM Personeller
SELECT Adi + ' ' + CAST(IseBaslamaTarihi AS nvarchar) FROM Personeller
```

## Tablo Oluştururken As komutu kullanma

```sql
USE Db_Education

CREATE TABLE Hesaplama(
BirinciSayi INTEGER,
IkinciSayi INTEGER,
Ortalama AS (BirinciSayi + IkinciSayi)/2,
Toplam AS (BirinciSayi + IkinciSayi)
)
```

## Select Komutunda Where Şartı

```sql
-- Personeller tablosunda şehri London olan verileri listele.
SELECT * FROM Personeller WHERE Sehir = 'London'

-- Personeller tablosunda bağlı çalıştığı kşi saıyısı 5 ten küçük olanları listele.
SELECT * FROM Personeller WHERE BagliCalistigiKisi < 5

-- Farklı Bir Select Kullanımı 
SELECT * FROM ( VALUES ('ali',70),('veli',60),('can',50) ) AS C(isim, agirlik) WHERE agirlik < 60

------- MANTIKSAL OPERATÖRLERİN KULLANIMI  -------

-- AND Operatörü
-- Personeller tablosunda şehri London ve ülkesi UK olanları listele.
SELECT * FROM Personeller WHERE Sehir = 'London' AND Ulke = 'UK'

-- OR Operatörü
-- Personeller tablosunda ünvan eki 'mr.' veya şehiri 'Seattle'  olanları listele.
SELECT * FROM Personeller WHERE UnvanEki = 'Mr.' OR Sehir = 'Seattle'

-- NOT Operatörü
-- Personeller tablosunda þehri London olmayan verileri listele.
SELECT * FROM Personeller WHERE NOT(Sehir = 'London')

------- CONDITIONS (KOŞULLAR) KULLANIMI -------



```










```sql

```
```sql

```
```sql

```
