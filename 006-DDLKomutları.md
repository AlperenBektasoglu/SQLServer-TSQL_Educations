# DDL(Data Definition Language) Komutları (CREATE / ALTER / DROP)

T-SQL de veritabanı nesnelerini yaratmamızı, bu nesneler üzerinde değişiklikler yapmamızı ve silmemizi sağlayan komutlar bu başlık altında incelenir. 

## CREATE Komutu

Veri tabanı nesnesi yaratmamızı sağlar. (databases,table,view,storedprocedure,trigger vs) 

Kullanım Prototipi: CREATE [NESNE] [NESNE_ADI]

Veritabanı Oluşturma:
```sql
-- Aşağıdaki komut veritabanını varsayılan ayarlarda oluşturur.
CREATE DATABASE  Db_Education

-- Aşağıdaki komut ile varsayılan ayarlar değiştirilerekte veritabanı oluşturulabilir.
CREATE DATABASE  Db_Education ON
(
NAME = 'Turkey', --Arka tarafta oluşturulacak veritabanı dosya ismi
filename = 'E:\Turkey.mdf',   -- Dikkat veritabanı dosyası .mdf uzantılı olmalıdır.
size = 10 ,
filegrowth = 5
)

-- NAME: Oluşturulacak veritabanının fiziksel ismini belirtilir.
-- FILENAME: Dosyanın dizini belirtilir.
-- SIZE: Veritabanının başlangıç boyutunu MB cinsinden ayarlanır.
-- FILEGROWTH: Veritabanının boyutu başlangıç boyutunu geçtiği durumda boyutun nekadar artması gerektiğini MB cinsinden belirtilir.	
```













```sql
```
```sql
```
```sql
```
