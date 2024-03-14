# Sql Temel Kavramlar

## Sql Server Genel İsimlendirme Kuralları

* İsimler sayı ile başlayamaz. Mutlaka harf ile ya da alt çizgi(_) ile başlamalıdır, sayıları isim içerisinde kullanabilirsiniz.
* İsimlendirmede Türkçe karakterler kullanılmamalıdır.
* İsimler boşluk karakteri içermemelidir. Birden fazla kelimeden oluşan isimlerde alt çizgi veya deve notasyonu kullanımlarından birini kullanabilirsiniz. (Örnek : ad_soyad ya da AdSoyad gibi.)
* İsimler SQL’de özel anlamı olan sembollerle (@, @@, #, ##, $) başlamamalıdır.
* İsimlendirmede T-SQL komutları değişken ismi olarak verilmemelidir (SELECT,UPDATE vb).
* İsimlendirmede kısa ve anlaşılması zor kelimeler yerine uzun ve anlaşılır değişken isimleri kullanılmalıdır.

## Genel Bilgiler

* Sql Server küçük büyük harfe duyarlı değildir. (Not Case Sensitive)
* Tek satırlı yorum: --
* Çok satırlı yarum: /*...*/
* Script'i seçtikten sonra çalıştırdığımızda seçtiğimiz script çalışır. Seçilmez ise bütün  scriptler çalışır.

## USE Anahtar Kelimesi

USE anahtar kelimesi hangi veri tabanında işlem yapacağımızı belirttiğimiz komuttur. 

```sql
USE Northwind
```

## Sql Komut Kategorileri

* DDL (Data Definition Language): CREATE / ALTER / DROP
* DML (Data Management Language): SELECT / INSERT / UPDATE / DELETE
* DCL (Data Control Language): GRANT / REVOK

## Sql Dosya Formatları

* Birincil Veri Dosyası: Veritabanının başlangıç ​​noktasıdır, ana veritabanı dosyalarıdır. Tablolar, stored prosedürler, viewler, tetikleyiciler , fonksiyonlar vb. veritabanı nesnelerindeki tüm veriler birincil veri dosyalarında saklanır. Her veritabanında sadece bir tane olmak zorundadır.  ".mdf" uzantılıdır.

* İkincil veri dosyaları: ".ndf" uzantılıdır. Birincil dosya haricindeki bütün veri dosyalarına ikincil veri dosyaları denir. Bazı veritabanlarında hiç olmayabildiği gibi bazı veritabanlarında birden fazla da olabilir.  İkincil veri dosyasını, birincil veri dosyasının depolandığından farklı bir fiziksel sürücüde depolamak da mümkündür. Bu performans anlamında çok ciddi avantajlar sağlayabilir. Örneğin en sık yazılan tablolar bir dosyada saklanır ve nispeten daha az yazılan tablolar başka bir dosyada saklanabilir.

* Log Dosyaları: ".ldf" uzantılıdır. Bir veritabanında birden fazla olabilir ama en az 1 tane olmak zorundadır. Log dosyalarında günlük bilgiler (verilerdeki değişiklik işlemleri ile ilgili transaction logları vs.)tutulur. Bu bilgiler daha sonra veritabanını kurtarmak için kullanılabilir.

## Sıklıkla Kullanılan Veri Tipleri

* char(n) -- String -- Uzunluğu değişmeyen sabit verileri saklar.Eğer n değeri 10 ise, daha kısa uzunlukta değer girilince kalan boşluğu kendi tamamlar ve öyle saklar.
* nchar -- String -- Sabit uzunlukta Unicode karakterleri saklar. Char tipinden farkı çoklu dil ve Unicode desteği olmasıdır. En fazla 4000 karakter.
* varchar(n) -- String --	Değişebilir uzunlukta verileri saklar. En fazla 8,000 karakter alır. 
* nvarchar -- String -- Değişebilir uzunlukta verileri saklar. varchar tipinden farkı çoklu dil ve Unicode desteği olmasıdır. En fazla 4000 karakter.
* text -- String -- Değişebilir uzunlukta karakterleri saklar. En fazla 2GB metin içerir.
* bit -- Boolean -- 0, 1 ve null değerini saklar.
* tinyint -- Number -- 0 ile 255 arasında değerleri saklar.
* smallint --	Number -- -32,768 ile 32,767 arasında değerleri saklar.
* int -- Number -- -2,147,483,648 ile 2,147,483,647 arasında değerleri saklar.
* bigint -- Number --	-9,223,372,036,854,775,808 ile 9,223,372,036,854,775,807 arasında değerleri saklar.
* datetime -- Date --	1 Ocak 1753 – 31 Aralık 9999. 3.33 milisaniye doğruluk hassasiyeti vardır.
* smalldatetime -- Date -- 1 Ocak 1900 – 6 Haziran 2079. 1 dakikalık doğruluk hassasiyeti vardır.
* date -- Date --	1 Ocak 0001 – 31 Aralık 9999. Sadece tarih içerir, saati saklamaz.

### Unicode Nedir? Unicode ile ASCII Farkları Nelerdir? 
Unicode’un geliştirilmesinin arında yatan temel neden ASCII (American Standart Code for Information Interchange) karakter kodlamasının daha gelişmiş ve stratejik bir sürümünün oluşturulabilmesidir. ASCII karakterler sadece İngilizce üzerinde etkili olurken, Unicode tamamen evrenseldir. Unicode’un farklı sürümleri sayesinde İbranice ve Arapça gibi kompleks diller başta olmak üzere Çince gibi karmaşık diller kolayca dijital ortamlara aktarılabilmektedir. Yalnızca diller değil, Unicode kodlaması sayesinde karmaşık semboller ve karakterler kolayca meydana getirilebilirler. Yapısal açıdan bilgisayarlar sayılar yardımıyla çalışırlar. Her karakter için hafızlarında bir sayı tutar ve bu sayı yardımıyla karakter, rakam veya sembolün oluşturulmasını sağlarlar.

## Uniqueidentifier Veri Tipi

Aldığı değerler, rakamlar ve harflerden oluşan çok büyük bir sayıdır. Bundan dolayı bu tipteki kolona aynı değerin birden fazla gelmesi neredeyse imkansızdır. O yüzden tekil bir veri oluşturmak için kullanılır. NEWID() fonksiyonu ile bu tip için rastgele değer üretilir.

```sql
CREATE TABLE OrnekTablo
(
Id INT PRIMARY KEY IDENTITY,
Kolon1 INT,
Kolon2 UNIQUEIDENTIFIER
)

INSERT OrnekTablo VALUES (10, NEWID())
```











