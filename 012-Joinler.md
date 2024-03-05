
# Joinler

## Inner Join Yapısı

İlişkili tabloları birleştirmek için kullanılır. Her iki tablodaki ortak kayıtları döndürür. İki kümenin kesişimidir.

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
```
 
