# Alt Sorgular

Sorgu içerisinde yazılan sorgulara alt sorgu denir.

```sql
-- Ücreti 3000 liranın altında olan personel numaralarının listelenmesi:
SELECT Perno FROM Personel WHERE Ucret < 3000

-- İşyerindeki ortalama ücretten daha fazla ücret alan personellerin numaralarının listelenmesi: 
SELECT Perno FROM Personel WHERE Ucret > ( SELECT AVG(Ucret) FROM Personel )

-- Muhasebe bölümündeki Ali’den daha fazla ücret alan kaç kişi kişi olduğunu bulmak için: 
SELECT Count(*) FROM Personel
WHERE Ucret > ( SELECT Ucret FROM Personel
                WHERE Ad=‘Ali’ AND Dept=‘MUH’ ) 

-- Satış bölümündeki Eda’dan daha kıdemli personellerin personel numaralarının listelenmesi:
SELECT Perno FROM Personel 
WHERE GorevBasTar < ( SELECT GorevBasTar FROM Personel
                      WHERE Ad=‘Eda’ AND Dept=‘SAT’ ) 

-- Muhasebe departmanında ki kişilerden daha yüksek ücret alan kişilerin isminin listelenmesi: (>ALL , <ALL, >=ALL , <=ALL)
SELECT Ad FROM Personel WHERE Ucret >ALL
( SELECT  Ucret FROM  Personel WHERE Dept="MUH" )

-- Muhasebe departmanında ki kişilerin en az birisinden yüksek ücret alan kişilerin isminin listelenmesi: (>ANY , <ANY, >=ANY , <=ANY, =ANY)
SELECT Ad FROM Personel WHERE Ucret >ANY
( SELECT  Ucret FROM  Personel WHERE Dept="MUH" )
```

```sql
-- Ali isimli müşterinin yapmış olduğu alışveriş kayıtlarının listelenmesi: 
SELECT * FROM Satis
WHERE Musno = ( SELECT MusNo FROM Musteri
                WHERE MusAd=‘Ali’ )

-- Bolu’lu müşterilerin yaptıkları alışveriş kayıtlarının listelenmesi: 
SELECT * FROM Satis
WHERE Musno IN ( SELECT MusNo FROM Musteri
                 WHERE MusSehir=‘Bolu’ )

-- Defter almış olan müşterilerin isimlerinin listelenmesi: 
SELECT MusAd FROM Musteri WHERE MusNo IN 
(SELECT Musno FROM Satis WHERE UrunNo = 
(SELECT  UrunNo FROM Urun WHERE Urunad=‘Defter’ ) )

-- Ortalama fiyatın altında, birim fiyata sahip olan ürünlere ait satış kayıtlarının listelenmesi: 
SELECT * FROM Satis WHERE UrunNo IN 
(SELECT UrunNo FROM Urun WHERE Fiyat < 
( Select AVG(Fiyat) FROM Urun ) )

-- Üretilmiş (tanımlanmış) ama hiç satışı yapılmamış olan ürün/ürünlerin listelenmesi: 
SELECT UrunNo FROM Urun
WHERE UrunNo NOT IN ( SELECT UrunNo FROM Satis )
```

