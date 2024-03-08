
# Indexes

## Index Nedir?

* Indexler hakkında klasik bir örnek olarak telefon rehberi verilebilir. Telefon rehberindeki kayıtların sıralı olmaması durumunda(index olmayan bir tablo) arayacağımız bir isim için(select sorgusu) tüm rehberi gezmemiz gerekecek. (Table Scan) İkinci durumda ise telefon rehberinin isme göre sıralı olduğunu düşünelim. (index olan bir tablo) Bu durumda biliyorum ki Mehmet M harfinde. Kabaca rehberin ortasına gelirim. Ve oradaki harfe bakarak ileriye ya da geriye gitmem gerektiğine karar veririm. Yani bir önceki duruma göre çok çok daha hızlı bir şekilde Mehmet’i bulabilirim. Bu şekilde aradığımız veri veya verilere, eleme yöntemi ile bir kaç adımda ulaşabiliriz. Bu örnekteki gibi verinin sıralı tutulmasını sağlayan yapılara index denir.
* İndexler tablolarda ki verileri fiziksel veya mantıksal olarak sıralar. 
* SQL Server açısından index kullanımının en önemli amacı, istenen bilginin daha az veri okuyarak daha kısa zamanda getirilmesini sağlamaktır.

## Indexlerin Çalışma Prensibi

Index’lerin nasıl çalıştığına değineceğiz. Ama öncelikle Sql Serverda verilerin nasıl tutulduğuna bakalım. Veriler fiziksel olarak veri dosyalarında(.mdf/.ndf uzantılı dosya) tutulur. SQL Server’de database’lere ait fiziksel dosya isim ve konumları master database ‘de kayıt edilir. Sql Server bu veri dosyalarını fiziksel olarak değil mantıksal olarak 8 KB’lık bloklara böler. Bu bloklara <Page> denir. Bundan dolayı bu veri dosyalarının ilk 8 KB’ı <page0>, bir sonraki 8 KB’ı <page1> şeklinde devam eder. Yani SQL Server her 1MB data için 128 adet page içerir. Sql Server, bir sorgu sonucu bir veriye ulaşırken, aslında tablodaki satırları okumaz bunun yerine page’leri okuyarak verilere ulaşır.





