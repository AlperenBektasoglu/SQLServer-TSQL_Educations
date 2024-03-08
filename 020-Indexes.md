
# Indexes

## Index Nedir?

* Indexler hakkında klasik bir örnek olarak telefon rehberi verilebilir. Telefon rehberindeki kayıtların sıralı olmaması durumunda(index olmayan bir tablo) arayacağımız bir isim için(select sorgusu) tüm rehberi gezmemiz gerekecek. (Table Scan) İkinci durumda ise telefon rehberinin isme göre sıralı olduğunu düşünelim. (index olan bir tablo) Bu durumda biliyorum ki Mehmet M harfinde. Kabaca rehberin ortasına gelirim. Ve oradaki harfe bakarak ileriye ya da geriye gitmem gerektiğine karar veririm. Yani bir önceki duruma göre çok çok daha hızlı bir şekilde Mehmet’i bulabilirim. Bu şekilde aradığımız veri veya verilere, eleme yöntemi ile bir kaç adımda ulaşabiliriz. Bu örnekteki gibi verinin sıralı tutulmasını sağlayan yapılara index denir.
* İndexler tablolarda ki verileri fiziksel veya mantıksal olarak sıralar. 
* SQL Server açısından index kullanımının en önemli amacı, istenen bilginin daha az veri okuyarak daha kısa zamanda getirilmesini sağlamaktır.

## Indexlerin Çalışma Prensibi

* Index’lerin nasıl çalıştığına değineceğiz. Ama öncelikle Sql Serverda verilerin nasıl tutulduğuna bakalım. Veriler fiziksel olarak veri dosyalarında(.mdf/.ndf uzantılı dosya) tutulur. SQL Server’de database’lere ait fiziksel dosya isim ve konumları master database ‘de kayıt edilir. Sql Server bu veri dosyalarını fiziksel olarak değil mantıksal olarak 8 KB’lık bloklara böler. Bu bloklara <Page> denir. Bundan dolayı bu veri dosyalarının ilk 8 KB’ı <page0>, bir sonraki 8 KB’ı <page1> şeklinde devam eder. Yani SQL Server her 1MB data için 128 adet page içerir. Sql Server, bir sorgu sonucu bir veriye ulaşırken, aslında tablodaki satırları okumaz bunun yerine page’leri okuyarak verilere ulaşır.

* 8 adet Page’in oluşturduğu gruba <Extent> denilir.

* Sql Server, bir veriyi indexsiz bir tabloya eklerken sıralı olarak diskte tutmaz ve eklenen veri rastgele boş olan bir page’e yazılır. Bir veri sadece bir page içinde olabilir. Bir veri arandığında; Sql Server tablonun kayıtlarına, pagelere bakarak sırayla erişir ve aranılan kayıt ile eşleştirir. Kayıt bulunsa bile eşleşebilecek başka kayıt var mı diye tüm kayıtlarda karşılaştırma işlemi yapar. Sql Server’ın yaptığı bu işleme Table Scan(Tablo Tarama) denir. Bu işlem tablodaki kayıt sayısına göre çok uzun zaman alabilir. Özetle Indexsiz bir tablodan veri arandığı zaman, verinin bulunması için Sql Server’ın arka tarafta table scan tarama yaptığını ve tablodaki veri miktarı arttığı zaman, table scan işleminin perfomansının(zaman açısından) kötüleştiğini söyleyebiliriz.

* Indexler B-tree(Balance Tree) yapısında çalışırlar.

## B-tree(Balance Tree) Yapısının Çalışma Mantığı

![Alternatif Metin](Assets/Screenshot20.png)

Tablodaki kolon veya kolonlara index tanımlandığı zaman, Sql Server o tablodaki verileri aşağıdaki gibi bir Balanced Tree yapısına göre organize eder. B-tree yapısı 3 leveldan oluşur. Tablomuzdaki bir kolonumuzda 1’den 200’e kadar değerler olsun ve bu kolon’a index koyalım;

* Root Level: B-tree yapısındaki en üst seviyedir. Index üzerinde arama yapıldığında arama buradan başlar ve ağacın alt tarafına doğru ihtiyacı olan veriyi bulmak için arama işlemi devam eder. Resimde gördüğünüz gibi Root Level Page’de 1’den 200’e kadar değerler bulunuyor.
* Intermediate Levels: Root Level Page’den sonra intermediate level page’ler gelir. Index verilen kolonun büyüklüğüne hiç olmayada bilir yahut bir veya birden fazlada intermediate level olabilir. Yukarıdaki resimde gördüğünüz örnekte 2 tane intermediate level var. Örneğin değeri 123 olan kaydı aradığımızı düşünelim. Önce root level’e geliyoruz. Ardından 101-200 arasındaki değerleri işaret eden intermediate level’deki page’e gidiyoruz. Ardından bu page’de bizi hemen altındaki 101-150 arasındaki değerleri işaret eden diğer intermediate level page’ e yönlendiriyor.
* Leaf Level: B-tree yapısının son seviyesidir. Örneğimize devam edersek, arama işlemi 101-150 arasındaki kayıtları işaret eden intermediate level page’e geldiğinde bu intermediate level’den 101-125 arasındaki leaf level page’e yönlenecektir. Leaf level, Index’in tipine göre(Clustered ya da Non Clustered) verinin kendisi ya da verinin yerini işaret eden pointer’ın olduğu level’dır. Veriye ulaşmak için her zaman Leaf Level’a kadar inmek gerekir.

Örnekte de görüldüğü gibi index olan bir kolonda arama yaparken, aradığımız veriyi daha az mantıksal okuma ile bulabiliriz. Ama index kullanılmasaydı, yani veriler yukarıdaki gibi ağaç yapısında organize edilmeseydi tüm kayıtlar gezilerek veriye ulaşılmaya çalışılacaktı.

## Heap Table
Sql Server’da heap table adında bir somut bir kavram yoktur. Bir tabloya heap denilmesi aslında onun üzerinde bir index tanımlı olup olmamasına bağlıdır. Sql Server bir veriyi indexsiz bir tabloya eklerken sıralı olarak diskte tutmaz ve veriler rastgele data page’lere yazılır. Bu şekilde olan tablolar heap table diye adlandırılır. Yani üzerinde clustered index olmayan tablolar heap table’dır diyebiliriz.

## Index Çeşitleri
Sql Server’da indexler temelde clustered ve non-clustered index olmak üzere ikiye ayrılır.

### Clustered Index

* Verilerin belirli bir kolona göre fiziksel olarak sıralanmasını sağlayan index’e clustered index ismi verilir. 
* Bir tablo fiziksel olarak sıralandığından tablo üzerinde sadece bir tane clustered index tanımlanabilir. Clustered index için seçilecek kolon sorgulardaki en fazla kullanılan kolon olmalıdır. Veriler, bu kolona göre fiziksel olarak sıralanacağından verilere çok hızlı erişilir. 
* Ayrıca seçilen kolonun çok değiştirilmeyen bir alan olması gerekir. Çünkü index’e ait kolonun değişmesi demek ilgili kolonun yeniden B-Tree yapısına göre organize olması yani fiziksel olarak yeniden sıralanması anlamına gelir. 
* B-tree yapısında leaf node’larda tutulan veri, verinin kendisi ise Clustered indextir. 
* Bir tabloya Primary Key eklendiği zaman, Prımary Key default olarak her zaman o tablonun clustered indexi olarak tanımlıdır. Özetle primary key aynı zamanda bir clustred indextir.

### Non-Clustered Index

* Bir tabloda clustured indeks olan kolon dışında farklı bir kolon üzerinden tekrar indeksleme yapılmak isteniyorsa non-clustured indeks kullanılır ve bunun için tekrar bir dengeli ağaç yapısı(balance tree) oluşturulur.
  
**Not:** Non-clustered index atamak için tabloda clustered index olmasına gerek yoktur.

* Non-Clustered Index veriyi fiziksel değil mantıksal olarak sıralar. 
* Bir kolonu non-clustered index olarak indexlediğinizde, Sql Server arka tarafta yeni bir tablo oluşturur. Non-clustered indekste verilere direkt erişilemez. 
* B-tree yapısında leaf node’larda tutulan veri, verinin nerede olduğu bilgisi(clustered index anahtarı yada heapte ki adresi) ise Non-Clustered indextir.
* Non-Clustered Indexler, clustered index veya Heap üstünden hızlı olarak kayıtlara erişim sağlamak için tanımlanırlar. Bir tabloda en fazla 249 tane Non-Clustered index olabilir.

**Not:** Unique non-clustered indexte, clustered index anahtarı, non-clustered indexin B-tree yapısının leaf seviyesine eklenir. Unique olmayan non-clustered indexte, clustered index anahtarı, non-clustered indexin B-tree yapısının bütün seviyelerine eklenir.

**Not:** Non-clustered index tanımlanırken eğer tabloda clustered index yoksa, leaf level’ın pagelerinde tutulan veri, heap table’ı gösteren non-clustered indexin satır tanımlayıcısıdır(RID). Heap table’da, verileri bulana kadar kaydı satır satır arayacaktır.

**Not:** Tabloda clustered index varsa, non-clustered indexin Satır Tanımlayıcısı (RID) clustered indexin anahtarına işaret eder.

## Index Tanımlama Yaklaşımları

Index tanımlarken en önemli nokta, çalışılan sistemin OLAP veya OLTP olduğudur. OLAP’lar okuma ağırlıklı sistemler olduğundan, index sayısının fazla olması işleri kolaylaştırır. OLTP’ler de daha çok Update, Insert, Delete işlemleri yoğun olduğu için index sayısının artması SQL Server’a yük getirir. Az olması tavsiye edilir. Ancak bir tablo için hiç index tanımlanmaması da tabloda fazlaca kayıt olduğu sürece performansı azaltır. Bri tabloda bir clustered ve bir tane de non-clustered olduğunu varsayalım. Bu tabloya 1 kayıt eklendiğinde aslında 3 ekleme yapılmaktadır. Bundan dolayı ındexler okuma da hızı getirir ama yazma konusunda yavaşlığa neden olur.

**Not:** Not: Sql Server index ihtiyacını aslında kendisi belirler. Bizim tanımlayacağımız index’leri kullanıp kullanmamaya Query optimizer aracılığı ile  kendisi karar verir.




