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
