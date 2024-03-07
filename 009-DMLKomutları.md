
# DML(Data Manipulation Language) Komutları (INSERT/ UPDATE/ Delete)

T-SQL de veritabanına veri eklemek, silmek veya değiştirmek için kullanılan komutlar bu başlık altında incelenir.

## Insert Komutu

Tablo içine veri eklemek için kullanılır.

Kurallar:
1. Tarih yazdığımızda Ay.Gün.Yıl şeklinde yazılır. SQL Server, kolona yerleştirirken Yıl.Ay.Gün şeklinde yerleştirir.
2. Değerler kolon sırasına göre girilmelidir.
3. Kolonlara girilen değerler kolonun veri tipi ile aynı olmalıdır.
4. Identity özelliğine sahip kolona veri girişi yapılamaz.
5. Tabloda seçilen kolonlara yada bütün kolonlara veri girişi yapılacak dediğimiz zaman verileri eksik girersek hata verecektir.

```sql
INSERT Personeller(Adi,SoyAdi) VALUES ('Alperen','Bektaþoðlu')

-- INSERT komutu INSERT INTO şeklindede yazılabilir. INTO eski yazılış biçimidir ve günümüzdede SQL Server destekliyor.
INSERT INTO Personeller(Adi,SoyAdi) VALUES ('Alperen','Bektaþoðlu')

-- Kolon belirtmediğimiz zaman: Bütün kolonlara veri girişi yapılmalıdır.
INSERT Personeller VALUES('Bektaşoğlu', 'Alperen', 'Siber Güvenlik Uzmanı', 'Mr.', '05.22.1997', GETDATE(), 'Istanbul', 'Istanbul', 'Marmara Bölgesi', '000000', 'TURKEY', '0000000000', NULL, NULL, NULL, NULL, NULL)

-- Veri eklerken NOT NULL olan olan kolonlara mutlaka deðer gönderilmelidir.
INSERT INTO Personeller(Adi) VALUES ('Alperen') -- Hata veririr.

-- Bir komut ile birden çok veri eklemek
INSERT Personeller(Adi,SoyAdi) VALUES ('AAAAA','AAAAA'),('BBBBB','BBBBB'),('CCCCC','CCCCC')
```

### Insert Komutu İle Select Sorgusu Sonucunda Gelen Verileri FarklI Bir Tabloya Kaydetme

**Not:** Select sorgusunda dönen kolon sayısı ve veri tipi ile insert işlemi yapılacak tablonun kolon sayısı ve veri tipi aynı olmalıdır.

```sql
CREATE TABLE AdSoyadPersoneller
(
Isim NVARCHAR(10),
SoyIsim NVARCHAR(20)
)

-- 1.Yol
INSERT AdSoyadPersoneller SELECT Adi,SoyAdi FROM Personeller

-- 2.Yol
SELECT Adi, SoyAdi INTO AdSoyadPersoneller FROM Personeller
```

## Update Komutu

Tablodaki verileri güncellemek için kullanılan anahtar kelimedir. 

```sql
CREATE TABLE DenemePersoneller
(
Isim NVARCHAR(10),
SoyIsim NVARCHAR(20),
PerNo INT,
Cinsiyet CHAR(5) CONSTRAINT Cinsiyet_CheckConstraint CHECK( Cinsiyet IN('Kadın','Erkek'))
)

UPDATE DenemePersoneller SET Isim = 'Merve'
UPDATE DenemePersoneller SET Isim = 'Alperen' WHERE SoyIsim = 'Bektaşoğlu'
UPDATE TOP(2) DenemePersoneller SET Isim = 'x'  -- TOP(n) komutu, yukarıdan n satırı ifade ediyor.
UPDATE DenemePersoneller SET Isim = 'Mahsuni', SoyIsim = 'Şerif' WHERE PerNo = 555
```

## Delete Komutu

Tablodan veri silmek için kullanılan anahtar kelimedir. 

```sql
USE Db_Education

CREATE TABLE DenemePersoneller
(
PerNo INT PRIMARY KEY IDENTITY (1,1),
Isim NVARCHAR(10),
SoyIsim NVARCHAR(20),
Cinsiyet CHAR(1) CONSTRAINT Cinsiyet_CheckConstraint CHECK( Cinsiyet IN('K','E')),
)

-- SoyIsim = 'bektaşoğlu' olan satırı silme
DELETE FROM DenemePersoneller WHERE SoyIsim = 'Bektaşoğlu'

-- Tablonun içindeki bütün verileri silme
DELETE FROM DenemePersoneller
```

## Truncate Komutu
Truncate komutu tablo yapısını değiştirmeden, tablo içinde yer alan tüm verileri tek komutla silmenizi saðlar. Truncate komutunu kullandığımızda Primary Key(Birincil Anahtar) değeri, tıpkı tabloyu ilk oluþturmuşsunuz gibi 1'den başlayacaktır. Delete Table komutu ile de bir tablo içindeki verileri silebilirsiniz, fakat indeks deðerleri kaldığı yerden başlayacaktır. Örneğin silinmeden önce tblogrenci tablomuzda ogrenciID alanı en son 100 deðerini almış ise delete ile sildiğinizde 101'den başlayacak, truncate ile sildiğinizde 1'den başlayacaktır.

Kullanım Prototipi: TRUNCATE TABLE [TABLO_ADI]

```sql
TRUNCATE TABLE DenemePersoneller
```
