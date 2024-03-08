# Backup / Restore

Simple Recovery Model

En temel kurtarma türü olup, küçük çaplı ve sadece sorgulama için kullanılan veritabanlarında kullanılır. Transaction loglar (Insert/Update/… gibi VT’de yapılan değişikliklere dair kayıtlar) kaydedilmez ve veritabanına ait transaction loglar aktif değilse silinir. Bu yüzden en son yedekten sonra yapılan değişiklikler kayıt altına alınmadığı için o aradaki değişiklikler geri alınamaz. 

Full Recovery Model

Bu modelde transaction loglar kaydedilir ve mevcut bir yedek geri yüklendiğinde veritabanındaki verilerle beraber tüm transaction loglar da geri yüklenir. Bu yüzden bu modelde son ana kadar yapılmış olan tüm değişiklikler kurtarılabilir veya veritabanının istenilen bir zamandaki haline geri dönülebilir. Veri kaybı riskinin yüksek olduğu veritabanlarında tercih edilir.

Bulk-Logged Recovery Model

Simple ve Full recovery modelleri arasındadır. Bulk işlemler (örneğin txt bir dosyadaki verilerin SQL veritabanına aktarılması vb.) dışındaki tüm işlemler loglanır. Bulk işlemi sırasında istenmeyen bir durum oluşursa bu süreçte yapılan değişiklikler geri alınamaz. Herhangi bir sorun durumunda veritabanının eski haline dönülmek istendiğinde bulk işlemler dışındaki tüm işlemler ve verilerin son durumu elde edilebilir.

## Veritabanının Recovery Modelinin Öğrenilmesi

