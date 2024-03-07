
# Triggers(Tetikleyiciler)

Triggerlar iki ye ayrılır:
1. DDL Trigger
2. DML Trigger

## DDL Trigger

Create, alter ve drop işlemleri sonucunda devreye giren yapılardır. Bu işlemler sonucunda veya sürecinde devreye girerler. Triggerlar çalışma mantığı açısından da ikiye ayrılırlar.

1. İşlem sonucunda çalışan triggerlar  -- AFTER/FOR
2. İşlemin yerine çalışan triggerlar   -- INSTEAD OF 

## DML Trigger

Bir tabloda insert, update ve delete işlemleri gerçekleştirildiğinde devreye giren yapılardır. Bu işlemler sonucunda veya sürecinde devreye girerler.

Inserted Table: Eğer bir tabloda insert işlemi yapılıyor ise arka planda işlemler, ilk önce RAM'de oluşturulan inserted isimli bir tablo üzerinde yapılır. Eğer işlemde bir problem yoksa inserted tablosundaki veriler fiziksel tabloya Insert edilir. İşlem bittiği zaman RAM'de oluşturulan bu inserted tablosu silinir. Bu anlatılan insert komutunun çalışma prensibidir.

Deleted Table: Eğer bir tabloda delete işlemi yapılıyor ise arka planda işlemler, ilk önce RAM'de oluşturulan deleted isimli bir tablo üzerinde yapılır. Eğer işlemde bir problem yoksa deleted tablosundaki veriler fiziksel tablodan silinir. İşlem bittiği zaman RAM'de oluşturulan bu deleted tablosu silinir. Bu anlatılan delete komutunun çalışma prensibidir.

**Not:** Eğer bir tabloda update işlemi yapılıyor ise RAM'de updated isimli bir tablo OLUŞTURULMAZ!SQL Server'da ki update mantığı önce silme(Delete), sonra eklemedir(Insert). Eğer bir tabloda update işlemi yapılıyor ise arka planda, RAM'de hem deleted hem de inserted tabloları oluşturulur ve işlemler bunlar üzerinden yapılır.

**Not:** Update işlemi yaparken; güncellenen kaydın orjinali deleted tablosunda, güncellendikten sonraki hali ise Inserted tablosunda bulunmaktadır.

**Not:** Deleted ve inserted tabloları, ilgili sorgu sonucu oluştukları için o sorgunun kullandığı kolonlara da sahip olur. Böylece deleted ve inserted tablolarından select sorgusu yapmak mümkündür.

