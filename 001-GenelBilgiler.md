# Genel Bilgiler

## Veri/Data Nedir?
İşlenmemiş (ham), dağınık haldeki bilgi topluluğudur. Veriler, anlamlı bir şekilde düzenlendiğinde ve işlendiğinde anlamlı bilgi ortaya çıkar.

## Veritabanı/Database Nedir?
Herhangi bir konuda birbiriyle ilişkili ve düzenli olarak tutulan veriler topluluğudur. 

## Veritabanı Yönetim Sistemi (VTYS)/Database Management System(DBMS) Nedir?
Veritabanı üzerindeki yönetim işlemlerinin (veritabanı oluşturma, tablo oluşturma, kayıt işlemleri, sorgulamalar, güvenlik ayarları, yedekleme ve bakım…) 
kullanıcı tarafındankolaylıkla yapılabilmesini sağlayan sistemlerdir. Piyasada farklı isimlerle birçok VTYS vardır. Bunlardan bazıları MS Access, Oracle, 
MySQL, Sybase, Paradox, MS SQL Server…

## VTYS’lerin Faydaları
* Veri tekrarı (Data Redundancy) önlenir. Bir tabloya girilmiş olan bir bilginin aynı veya farklı tablolara tekrar girilmesi önlenmiş olur.
* Veri tutarlılığı (Data Consistency) sağlanır. Bir bilgi değiştiğinde, o bilginin bulunduğu diğer tablolarda da otomatik olarak değişmesi gerekir.
Bu işlem yapılmadığında kayıtlar bozulmaya / farklılaşmaya başlar.
* Veri paylaşımı (Data Concurrency) sağlanır. Tutulmakta olan verilerin, çok sayıda istemcinin aynı anda kullanımına sunulabilmesi. VTYS, aynı anda birden
fazla kullanıcının aynı veri üzerinde değişiklik yapmak istediğinde yetki üstünlüğü veya bağlantı önceliği gibi kriterlere göre sıralama yapar.
* Veri bütünlüğü (Data Integrity) sağlanır. Bir verinin birden fazla tablo gibi parçalara bölündüğü durumlarda tüm verilerin bütün olarak kullanılmasını sağlaması
ya da bir veri silindiğinde o verinin ilişkili olduğu tüm tablolardan da silinmesinin sağlanması.
* Veri güvenliği (Data Security) sağlanır. Yetkisiz kullanıcıların veritabanına bağlanmasının önlenmesi, bağlanan kullanıcıların da yetkileri dahilindeki verileri
görebilmesi, veriler üzerinde işlem yapabilmeleri gibi ayarlar.
* Veri bağımsızlığı (Data Independence) sağlanır. Veritabanının fiziksel yapısı kullanıcılardan gizlenir. Çeşitli programlama dilleri ile bağlanılarak veriler
üzerinde işlemler yapılabilmesine imkan verilir.

## Veri Modelleri/Data Model
Veritabanlarının temel görevi, istenilen bilgileri saklamak olmasına rağmen yapısal olarak farklılıklar içermektedir. Verilerin depolanması, işlenmesi,
veriler arasındaki ilişkilerin kurulması gibi yapısal detaylar Veri Modeli olarak isimlendirilir. Her VTYS, verilerin mantıksal olarak düzenlenmesi için bir
veri modeli kullanır. 

### İlişkisel Veri Modeli (Relational Data Model)
Günümüzde en yaygın kullanılan veri modelidir. Ortak özelliğe sahip veriler tablolar aracılığı ile tutulurlar. Tablolar satır ve sütunlardan oluşur. 
Veriler ve ilişkiler tablolar üzerinde tanımlanır ve tüm bilgiler görülebilecek şekildedir. İlişkiler kurulurken birincil anahtar ve yabancıl anahtarlar kullanılır.










