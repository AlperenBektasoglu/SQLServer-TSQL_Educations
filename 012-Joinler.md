# Joinler

## Inner Join Yapısı

İlişkili tabloları yan yana birleştirmek için kullanılır. Her iki tablodaki ortak kayıtları döndürür. İki kümenin kesişimidir.

```sql
CREATE TABLE Ders
(
DersID INT PRIMARY KEY,
DersAd NVARCHAR(10)
)

CREATE TABLE Ogrenci
(
OgrNo INT UNIQUE,
OgrAd NVARCHAR(10),
DersID INT FOREIGN KEY REFERENCES Ders(DersID)
)

SELECT * FROM Ders D INNER JOIN Ogrenci O ON D.DersID = O.DersID
SELECT * FROM Ders D INNER JOIN Ogrenci O ON O.DersID = D.DersID
SELECT * FROM Ogrenci O INNER JOIN Ders D  ON O.DersID = D.DersID

USE Northwind

-- Örnek 1
SELECT * FROM Personeller INNER JOIN Satislar ON Personeller.PersonelID = Satislar.PersonelID

-- Örnek 2
SELECT * FROM Satislar INNER JOIN Personeller ON Personeller.PersonelID = Satislar.PersonelID

-- Tablolara Alias(AS) tanımlama
SELECT * FROM Personeller P INNER JOIN Satislar S ON P.PersonelID = S.PersonelID

-- Inner join de Where Şartı Yazma
SELECT * FROM Personeller P INNER JOIN Satislar AS S ON P.PersonelID = S.PersonelID
WHERE UnvanEki = 'Mr.' AND SoyAdi ='King'

-- Örnekler

-- Hangi personel hangi satışları yapmıştır? (Personeller, Satislar)
SELECT * FROM Personeller INNER JOIN Satislar ON Personeller.PersonelID = Satislar.PersonelID

-- Hangi ürün hangi kategoridedir? (Urunler, Kategoriler)
SELECT UrunAdi, KategoriAdi FROM Urunler INNER JOIN Kategoriler ON Urunler.KategoriID = Kategoriler.KategoriID

-- Beverages kategorisinde kaç ürün vardır? (Urunler, Kategoriler)
SELECT COUNT(*) FROM Urunler INNER JOIN Kategoriler ON Urunler.KategoriID = Kategoriler.KategoriID WHERE KategoriAdi = 'Beverages'

-- Seafood kategorisindeki ürünlerin listesi nedir? (Urunler,Kategoriler)
SELECT UrunAdi FROM Urunler AS U INNER JOIN Kategoriler AS K ON U.KategoriID = K.KategoriID WHERE KategoriAdi = 'Seafood'
```

### Inner Join İle Birden Fazla Tabloyu Birleştirme

![Alternatif Metin](Assets/Screenshot15.png)

```sql
-- 1997 Yılından sonra Nancy kişisinin satış yaptığı firmaların isimleri nelerdir? (Personeller, Satışlar, Musteriler)
SELECT * FROM Personeller P
INNER JOIN Satislar S ON P.PersonelID = S.PersonelID
INNER JOIN Musteriler M ON S.MusteriID = M.MusteriID
WHERE P.Adi = 'Nancy' AND YEAR(S.SatisTarihi) > 1997
```

![Alternatif Metin](Assets/Screenshot16.png)

```sql
-- Limited şirket olan tedarikçilerden alınmış seafood kategorisindeki ürünlerin toplam satış tutarı nedir? (Kategoriler, Urunler, Tedarikciler)
SELECT SUM(U.HedefStokDuzeyi * U.BirimFiyati) FROM Kategoriler K
INNER JOIN Urunler U ON K.KategoriID = U.KategoriID
INNER JOIN Tedarikciler T ON U.TedarikciID = T.TedarikciID
WHERE T.SirketAdi LIKE '%Ltd.%' AND K.KategoriAdi = 'Seafood'
```

### Recursive İlişki

Aynı tabloyu ilişkisel olarak inner join ile birleştirme:

```sql
-- Personelleri ve Personellerin bağlı olarak çallıştığıtı kişilerin isimlerini listele.
SELECT * FROM Personeller P1
INNER JOIN Personeller P2
ON P1.BagliCalistigiKisi = P2.PersonelID
```

## Left Outher Join Yapısı

İlişkili tabloları yan yana birleştirmek için kullanılır. JOIN ifadesinin solundaki tablodan bütün kayıtları getirir, sağındaki tablodan sadece eşleşen kayıtları getirir ve oluşan tabloda boş olan kolonlara "NULL" değeri atar.

**Not:** Eskiden Joinler outher kelimesi ile kullanılırdı ama günümüzde outher kelimesini kullanmaya gerek yok.

```sql
USE Db_Education

SELECT * FROM Ders D LEFT OUTER JOIN Ogrenci O ON D.DersID = O.DersID
SELECT * FROM Ders D LEFT JOIN Ogrenci O ON D.DersID = O.DersID
SELECT * FROM Ogrenci O LEFT JOIN Ders D ON O.DersID = D.DersID
```

## Right Outher Join Yapısı

İlişkili tabloları yan yana birleştirmek için kullanılır. Join ifadesinin sağındaki tablodan bütün kayıtları getirir, solundaki tablodan sadece eşleşen kayıtları getirir ve oluşan tabloda boş olan kolonlara "NULL" değeri atar.

```sql
SELECT * FROM Ders D RIGHT OUTER JOIN Ogrenci O ON D.DersID = O.DersID
SELECT * FROM Ders D RIGHT JOIN Ogrenci O ON D.DersID = O.DersID
SELECT * FROM Ogrenci O RIGHT OUTER JOIN Ders D ON O.DersID = D.DersID
```

## Full Outher Join Yapısı

İlişkili tabloları birleştirmek için kullanılır. Join ifadesinin iki tarafındaki tablolarda eşleşen eşleşmeyen ne varsa getirir ve oluşan tabloda boş olan kolonlara "NULL" değeri atar.

```sql
SELECT * FROM Ders D  FULL OUTER JOIN Ogrenci O ON D.DersID = O.DersID
SELECT * FROM Ders D  FULL JOIN Ogrenci O ON D.DersID = O.DersID
SELECT * FROM Ogrenci O FULL JOIN Ders D ON O.DersID = D.DersID
```

## Cross Join Yapısı

İki tablonun kartezyen çarpımını alır. Kısacası iki tablonun kombinasyonunu alır.

Kartezyen Çarpımı: A ve B herhangi iki küme olsun. Birinci bileşeni A' dan, ikinci bileşeni B' den alınarak oluşturulabilecek tüm sıralı ikililerin kümesine, A ile B' nin kartezyen çarpımı denir ve A x B biçiminde gösterilir. Buna göre;

Örnek: Aynı futbol takımında oynayan Ali, Sertaç ve Tamer, 7, 10 ve 11 numaralı formaları giyebilirler.

A kümesi A = { Ali , Sertaç , Tamer }

B kümesi B = { 7 , 10 , 11 }

A X B = { (Ali, 7 ), (Ali, 10), (Ali, 11 ), (Sertaç,7 ), (Sertaç,10 ), (Sertaç,11 ), (Tamer, 7 ), (Tamer, 10 ), (Tamer, 11 ) }

```sql
USE Northwind

SELECT COUNT(*) FROM Personeller -- 9 Satır
SELECT COUNT(*) FROM Bolge --4 Satır

SELECT * FROM Personeller P CROSS JOIN Bolge B --36 Satır
```

**Not:** Cross join yapısı kullanıldığında where şartı yazılmaz. Tablolar cross join ile birleştirildiğinde oluşan yeni tablo sonuç olarak kabul edilir.

## Join Yapısında Group By Komutu Kullanma

```sql
-- Beverages kategorisine sahip ürünlerin sayısı nedir? (Kategoriler, Urunler)
SELECT  K.KategoriAdi , COUNT(U.UrunAdi) FROM Urunler AS U INNER JOIN Kategoriler AS K ON U.KategoriID = K.KategoriID
WHERE K.KategoriAdi = 'Beverages'
GROUP BY KategoriAdi

-- Adının baş harfi 'M' olan ve satış adedi 50 den fazla olan personellerin adını, soyadını ve toplam satış miktarını bul. (Personeller, Satislar)
SELECT P.Adi + ' ' + P.SoyAdi AS [İsim Soyisim] , COUNT(S.SatisID) FROM Personeller P INNER JOIN Satislar S ON P.PersonelID  = S.PersonelID
WHERE P.Adi LIKE 'M%'
GROUP BY P.Adi + ' ' + P.SoyAdi
HAVING COUNT(S.SatisID) > 50

-- Her personelin toplam satış miktarını bulun? (Personeller, Satislar)
SELECT P.Adi , P.SoyAdi , COUNT(S.SatisID) FROM Personeller P INNER JOIN Satislar S
ON P.PersonelID = S.PersonelID
GROUP BY P.Adi , P.SoyAdi
ORDER BY COUNT(S.SatisID) DESC

-- En çok satış yapan personel kimdir? (Personeller, Satislar)
SELECT TOP 1 P.Adi , P.SoyAdi , COUNT(S.SatisID) FROM Personeller P INNER JOIN Satislar S
ON P.PersonelID = S.PersonelID
GROUP BY P.Adi , P.SoyAdi
ORDER BY COUNT(S.SatisID) DESC

-- Adında 'A' harfi geçen  personellerin SatisID i 1000 den büyük olan satışların toplam tutarını(miktar * birimfiyat) ve bu satışların hangi tarihte gerçekleştiğini listeleyin. (Personeller,Satislar,[Satis Detaylari])
SELECT P.Adi , P.SoyAdi ,SUM(SD.BirimFiyati * SD.Miktar) , S.SatisTarihi FROM Personeller P
INNER JOIN Satislar S ON P.PersonelID = S.PersonelID
INNER JOIN [Satis Detaylari] SD ON S.SatisID = SD.SatisID
WHERE P.Adi LIKE '%a%'AND S.SatisID > 1000

-- Toplu Kullanım
SELECT P.Adi+' '+P.SoyAdi AS [İsim Soyisim] , COUNT(S.SatisID) FROM Personeller P
INNER JOIN Satislar S ON P.PersonelID  = S.PersonelID
WHERE P.Adi LIKE 'M%'
GROUP BY P.Adi + ' ' + P.SoyAdi
HAVING COUNT(S.SatisID) > 50 ORDER BY COUNT(S.SatisID) ASC
```

## Intersect Komutu

Intersect komutu iki farklı sql sorgusunun kesişim noktalarını bulur. Örneğin A ve B adında iki farklı küme düşünelim. Intersect komutu A ve B kümelerinin ikisinde de bulunan kayıtları listeleyecektir. İki tablodaki bütün kolonlar intersect komutu ile kıyaslanabilmesi için iki tablonunda kolon isimlerinin, kolon sayılarının ve kolon veri tiplerinin aynı olması gerekir.

```sql
USE Db_Education

CREATE TABLE tablo1(
Numara int,
Ad NVARCHAR(20),
Soyad NVARCHAR(20),
)

CREATE TABLE tablo2(
Numara int,
Ad NVARCHAR(20),
Soyad NVARCHAR(20),
)

INSERT tablo1 VALUES (1, 'Safiye', 'Bektaşoğlu'), (2, 'Alperen', 'Bektaşoğlu'), (3, 'Gökçen', 'Bektaşoğlu'), (4,'Abdullah','Bulut')
INSERT tablo2 VALUES (1, 'Mine', 'Boduroğlu'), (2, 'Esin', 'Boduroğlu'), (3, 'Hande', 'Boduroğlu'), (4,'Abdullah','Bulut')

SELECT * FROM tablo1 INTERSECT SELECT * FROM tablo2

-- Kolon Uzerinden Kıyaslama Yapma
SELECT Ad FROM tablo1 INTERSECT SELECT Ad FROM tablo2
SELECT Ad, Soyad FROM tablo1 INTERSECT SELECT Ad, Soyad FROM tablo2

USE Northwind

SELECT * FROM Urunler WHERE KategoriID = 1 INTERSECT SELECT * FROM Urunler WHERE TedarikciID = 1
```

## Except Komutu

Except komutu iki farklı sql sorgusundan birinin sonuç kümesinde olup diğerinin sonuç kümesinde olmayan kayıtları listeler. Örneğin A ve B adında iki farklı küme düşünelim. Except komutu A kümesinde bulunup B bulunmayan veyahut B kümesinde bulunup A da bulunmaya kayıtları listeleyecektir. İki tablodaki bütün kolonlar except komutu ile kıyaslanabilmesi için iki tablonunda kolon isimlerinin, kolon sayılarının ve kolon veri tiplerinin aynı olması gerekir.

```sql
USE Db_Education

Select Ad From tablo1 -- tablo1 de olup tablo2 de olmayan
Except
Select Ad From tablo2

Select Ad From tablo2 -- tablo2 de olup tablo1 de olmayan
Except
Select Ad From tablo1
```

## Union Komutu

Birden fazla select sorgusu sonucunu tek seferde alt alta göstermemizi sağlar. Joinler yan yana, Union alt alta tabloları birleştirir. Joinlerde belirli(ilişkisel) kolon üzerinden birleştirme yapılırken, union da böyle bir durum söz konusu değildir. Dikkat edilmesi gerekenler:

1. Union sorgusu sonucu oluşan tablonun kolon isimleri, üstteki tablonun kolon isimlerinden oluşur.
2. Üstteki sorgudan kaç kolon çekilmiş ise alttaki sorgudan da o kadar kolon çekilmek zorundadır.
3. Üstteki sorgudan çekilen kolonların tipleri ile alttaki sorgudan çekilen kolonların tipleri uyumlu olmalıdır.
4. Union, tekrarlı kayıtları getirmez.

```sql
USE Northwind

SELECT Adi, SoyAdi FROM Personeller
SELECT MusteriAdi, MusteriUnvani FROM Musteriler

SELECT Adi, SoyAdi FROM Personeller
UNION
SELECT MusteriAdi, MusteriUnvani FROM Musteriler

-- İkiden fazla tabloyu alt alta birleştirme
SELECT Adi, SoyAdi FROM Personeller
UNION
SELECT MusteriAdi, MusteriUnvani FROM Musteriler
UNION
SELECT SevkAdi, SevkAdresi FROM Satislar
```

**Not:** Union'da kullanılan tablolara kolon eklenebilir. Dikkat edilmesi geren nokta, yukarıdaki kurallar çerçevesinde eklenmelidir. Yani eklenecek kolon sayısı ve kolon tipleri aynı olmalıdır.

```sql
SELECT Adi, SoyAdi, 'Personel' FROM Personeller
UNION
SELECT MusteriAdi, MusteriUnvani, 'Müşteri' FROM Musteriler
```

## Union All Komutu

Union tekrarlı kayıtları getirmez. Tekrarlı kayıtları getirmek için Union All komutu kullanılır.

```sql
SELECT Adi, SoyAdi FROM Personeller
UNION
SELECT Adi, SoyAdi FROM Personeller

SELECT Adi, SoyAdi FROM Personeller
UNION ALL
SELECT Adi, SoyAdi FROM Personeller
```
