# Temel Sql Komutları

## Top Komutu

SELECT TOP komutu, çağırılacak verilerin sayısını belirtmek için kullanılır. Tüm veritabanı sistemleri SELECT TOP komutunu desteklemez. MySQL sınırlı sayıda veri seçmek için LIMIT komutunu desteklerken, Oracle ROWNUM komutunu kullanır.

```sql
USE Northwind
SELECT TOP 3 * FROM Personeller
```

### Top Komutu İle Update İşlemi

```sql
USE Db_Education
UPDATE TOP(2) DenemePersoneller SET Isım = 'Boş'
```

### Top Komutu İle Delete İşlemi

```sql
DELETE TOP(5) FROM DenemePersoneller
```

## Distinct Komutu

Bir kolondaki benzer olan verileri teke indirmemizi sağlayan komuttur.

```sql
USE Northwind
SELECT Sehir FROM Personeller
SELECT DISTINCT Sehir FROM personeller
SELECT COUNT(DISTINCT Sehir) FROM personeller
```

## Group By Komutu

Genel Formül: SELECT Listelenecek_alanlar, fonksiyonlar FROM Tablo_adı WHERE şart veya şartlar GROUP BY Gruplandırılacak_alanlar

Kolonlardaki satırların gruplanmasını sağlar. Group by en çok aggregate fonksiyonlarla(max, min, avg, count, sum) beraber kullanılır. Fonkisiyonlar kullanılmak zorunda değildir. Group by da "Listelenecek alanlar", "Gruplandırılıcak alanlar" içinde olmak zorundadır. Gruplandırılıcak alanların hepsi listelenmek zorunda değildir.

```sql
-- Örnek 1
SELECT * FROM Urunler
SELECT KategoriID , COUNT(*) FROM Urunler GROUP BY KategoriID

-- Örnek 2
SELECT KategoriID FROM Urunler GROUP BY KategoriID

-- Örnek 3
SELECT KategoriID , TedarikciID , COUNT(*) FROM Urunler GROUP BY KategoriID , TedarikciID

-- Örnek 4
SELECT * FROM Satislar
SELECT PersonelID , SUM(NakliyeUcreti) FROM Satislar GROUP BY PersonelID
```

Group By İşleminde Where Şartı Yazma: Where komutu Group By komutundan önce yazılır. Where komutu kolonlardaki şartlar için kullanılır.

```sql
SELECT KategoriID , COUNT(*) FROM Urunler WHERE KategoriID > 5 GROUP BY KategoriID
```

## Having Komutu

Group By komutundan sonra yazılır ve her zaman Group By ile birlikte kullanılır. Having komutu aggregate fonksiyonlarında ki şartlar için yazılır.

```sql
-- Örnek 1
SELECT KategoriID , COUNT(*) FROM Urunler WHERE KategoriID < 5 GROUP BY KategoriID HAVING COUNT(*) > 10

-- Örnek 2
SELECT PersonelID , SUM(NakliyeUcreti) FROM Satislar GROUP BY PersonelID HAVING SUM(NakliyeUcreti) > 5000
```

## With Rollup Komutu

Group By ile gruplandırılmış veri kümesinde ara toplam alınmasını sağlar.

```sql
-- Örnek 1 - Aşağıdaki örnekte satış id bazında SUM(Miktar) değerleri bulunduktan sonra, SUM(Miktar) değerlerini toplayıp(ara toplam) oluşan ara toplamı yeni satır olarak sonuç kümesine ekler.
SELECT SatisID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID WITH ROLLUP

-- Örnek 2 - Aşağıdaki örnekte satış id ve ürün id'lere göre veriyi grupladıktan sonra satış id bazında oluşan SUM(Miktar) değerlerini toplayıp (ara toplam), oluşan ara toplamı yeni satır olarak sonuç kümesine ekler.
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH ROLLUP

-- Örnek 3 (With Rollup komutunun Having komutu ile kullanılması)
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH ROLLUP HAVING SUM(Miktar) > 100
```

## With Cube Komutu

With Cube ifadesinde verilen tüm sütunların birbirleri olan tüm kombinasyonları için toplamları gösteren bir sonuç kümesi oluşturur. Group By ifadesi ile iki kolona göre gruplandığını varsayarsak, Cube ifadesi 1.kolon ve 2.kolon için oluşan tüm kombinasyonlar için bir ara toplam getiriyor. 1.kolon için 2.kolon ile birlikte oluşacak tüm kombinasyonlara bir ara toplam, 2.kolon için 1.kolon ile birlikte oluşacak tüm kombinasyonlara bir ara toplam, ayrıca sadece 1.kolon bazında toplam ve sadece 2.kolon bazında toplam ve son olarakta genel toplam döndürüyor.

```sql
-- Örnek 1
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH CUBE

-- Örnek 2
SELECT SatisID, UrunID, BirimFiyati, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID, BirimFiyati WITH CUBE

-- Örnek 3 (With Rollup komutunun Having komutu ile kullanılması)
SELECT SatisID, UrunID, SUM(Miktar) FROM [Satis Detaylari] GROUP BY SatisID, UrunID WITH CUBE HAVING SUM(Miktar) > 100
```

Kaynak: [Link](https://fatihsariyildiz.wordpress.com/2011/02/27/sql-cube-rollup-kullanimi/)

## Order By Komutu

Kayıtları belirtilen alanda büyükten küçüğe veya küçükten büyüğe göre sıralar. Defaul olarak küçükten büyüğe sıralanır. (ASC)

1. ASC (Ascending) --> Küçükten büyüğe sıralama
2. DESC (Descending) --> Büyükten küçüğe sıralama

```sql
SELECT * FROM [Satis Detaylari] ORDER BY UrunID

SELECT * FROM [Satis Detaylari] ORDER BY UrunID ASC

SELECT * FROM [Satis Detaylari] ORDER BY UrunID DESC

SELECT UrunID, BirimFiyati FROM [Satis Detaylari] ORDER BY BirimFiyati DESC

SELECT * FROM [Satis Detaylari] ORDER BY İndirim ASC , Miktar DESC

-- ORDER BY Komutunda WHERE Şartı Yazma
SELECT * FROM [Satis Detaylari] WHERE Miktar >= 100 ORDER BY UrunID ASC
```

## With Ties komutu

WITH TIES, TOP ve ORDER BY ile birlikte kullanılır. ORDER BY ile sıralanan sonuçlarda son kayıt ile aynı değerde olan kayıtların da listelenmesini sağlar. Bu durumda sonuç belirtilen n sayısından daha fazla olabilir. WITH TIES sadece ORDER BY ile birlikte kullanılmaktadır.

```sql
-- Aşağıdaki yapıda “Satis Detayları” adlı tablodan ilk 6 veri gelecektir. Ancak bu verilerden sonuncu olanı “10250” nolu kayıtın iki adeti dışarda kalmaktadır.Bu ikisinide getirmek için “WITH TIES” yapısını kullanmaktayız. With Ties order by ile sıralanmış kolonda, en son kayda bakar, eğer onun altında aynı nolu kayıt varsa onu da getirir. Bu yapıyı kullanabilmek için tek şart order by kullanılmalıdır.

SELECT TOP 6 * FROM [Satis Detaylari] ORDER BY SatisID

SELECT TOP 6 WITH TIES * FROM [Satis Detaylari] ORDER BY SatisID
```

## Toplu Kullanım

```sql
SELECT KategoriID, TedarikciID, COUNT(*) FROM Urunler WHERE KategoriID < 5
GROUP BY KategoriID, TedarikciID HAVING COUNT(*) < 4 ORDER BY KategoriID DESC
```

## Partition By Kullanımı

OVER() fonksiyonu ile iç içe, gruplama yapabilmek için PARTITION BY fonksiyonunu kullanılır. PARTITION BY() ile GROUP BY arasındaki en temel fark, PARTITION BY sonuç olarak gösterilen satırların sayısını azaltmaz yani her satırdaki işlemi gruplanmış bir şekilde ayrı ayrı gösterir. GROUP BY hatırlandığı üzere satırlar üzerinde genel bir işlem yapar ve sonucu o şekilde gösterir dolayısı ile sonuç olarak gösterilen satır sayısı azalabilir. PARTITION BY, GROUP BY ile aynı mantıkta çalışır ama tablodaki her satıra bir kolon ekleyerek gruplama sonucunu o kolonda gösterir.

```sql
-- Örnek 1: Aşağıdaki sorgu sonucu gelen satır sayısı, WEBOFFERS tablosundaki kayıt sayısı ile aynıdır.
SELECT
BRAND,
RAISEPRICE,
MIN(RAISEPRICE) OVER(PARTITION BY BRAND) AS MIN_FIYAT,
ROUND(AVG(RAISEPRICE) OVER(PARTITION BY BRAND),0) ORTALAMA_FIYAT,
MAX(RAISEPRICE) OVER(PARTITION BY BRAND) MAX_FIYAT
FROM WEBOFFERS

-- Örnek 2
SELECT ID,
BRAND,
YEAR_,
COUNT(ID) OVER(PARTITION BY YEAR_) YIL_ADET,
AVG(PRICE) OVER(PARTITION BY BRAND) ORTALAMA_FIYAT
FROM WEBOFFERS
```

Birden fazla sütuna göre PARTITION BY içinde gruplama yapmak mümkündür.

```sql
SELECT ID,
BRAND,
CASETYPE,
KM,
AVG(KM) OVER (PARTITION BY BRAND,CASETYPE) AS ORTALAMA_KM
FROM WEBOFFERS
ORDER BY ORTALAMA_KM DESC
```

Detaylı incelemek için <a href="https://mehmetali-kaya.medium.com/sql-i̇le-window-functions-over-ve-partition-by-eafe43a9162e"> tıklayın </a>
