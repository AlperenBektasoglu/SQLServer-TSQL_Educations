# DDL Komutları

## DDL(Data Definition Language) Komutları (CREATE / ALTER / DROP)

T-SQL de veritabanı nesnelerini yaratmamızı, bu nesneler üzerinde değişiklikler yapmamızı ve silmemizi sağlayan komutlar bu başlık altında incelenir.

## CREATE Komutu

Veritabanı nesnesi(databases,table,view,storedprocedure,trigger vs) yaratmamızı sağlar.

Kullanım Prototipi: CREATE [NESNE] [NESNE_ADI]

1. Veritabanı Oluşturma:

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

**Not:** Sadece veritabanı oluşturulduğunda log dosyasıda varsayılan ayarlarda arka tarafta oluşturulur. Aşağıdaki örnek ile varsayılan ayarlar değiştirilerekte log dosyası oluşturulabilir.

```sql
CREATE DATABASE  Db_Education ON
(
NAME = 'Turkey',
FILENAME = 'E:\Turkey.mdf',   --Dikkat veritabanı dosyası .mdf uzantılı olmalıdır.
SIZE = 10 ,
FILEGROWTH = 5
)
LOG ON
(
NAME = 'TurkeyLog',
FILENAME = 'E:\TurkeyLog.ldf',   --Dikkat log dosyası .ldf uzantılı olmalıdır.
SIZE = 10 ,
FILEGROWTH = 5
)
```

2. Tablo Oluşturma:

```sql
CREATE TABLE Ogrenci
(
[Ogrenci No] VARCHAR(4),    -- Değişken ismi boşluk içeriyorsa köşeli parantez kullanılır.
OgrenciAd VARCHAR(10),
OgrenciSoyad VARCHAR(20),
OgrenciTCNo CHAR(11) NOT NULL,
OgrenciBilgi TEXT NULL
)
```

## ALTER Komutu

Daha önceden oluşturulmuş olan bir veritabanı nesnesinin(databases,table,view,storedprocedure,trigger vs) yapısal özelliklerinin değiştirilmesi için kullanılan komuttur.

Kullanım Prototipi: ALTER [NESNE] [NESNE_ADI] ...

1. ALTER İle Veritabanı Güncelleme:

```sql
ALTER DATABASE  Db_Education  -- Fiziksel veritabanı ismini "Turkey" ve veritabanının başlangıç boyutunu 20M yapar.
MODIFY FILE  -- MODIFY FILE komutu veritabanında güncelleme yapmak için kullanılır.
(
NAME = 'Turkey',
SIZE = 20
)
```

2. ALTER İle Tabloya Sütun Ekleme:

```sql
ALTER TABLE Ogrenci ADD NotOrt FLOAT
ALTER TABLE Ogrenci ADD VizeNot VARCHAR(3) , FinalNot VARCHAR(3)
```

3. ALTER İle Tablodaki Bir Kolonun Veri Tipini Değiştirme:

```sql
ALTER TABLE Ogrenci ALTER COLUMN OgrenciAd VARCHAR(20)
```

4. Tablodaki Bir Kolonu ALTER Komutu İle Silme:

```sql
ALTER TABLE Ogrenci DROP COLUMN OgrenciBilgi
```

**Not:** Tablo ve kolon adlarını güncelleme işlemi SP_RENAME ile yapılır.

```sql
-- Tablo adını güncelleme
SP_RENAME 'Ogrenci','OgrenciBilgileri'

-- Kolon adını güncelleme
SP_RENAME 'OgrenciBilgileri.OgrenciAd','OgrenciAdYeni','column'
SP_RENAME 'OgrenciBilgileri.[Ogr No]','OgrNo','column'
```

## DROP Komutu

Daha önceden oluşturulmuş olan bir veritabanı nesnesinin(databases,table,view,storedprocedure,trigger vs) silinmesi için kullanılan komuttur.

Kullanım Prototipi: DROP [NESNE] [NESNE_ADI]

1. DROP Komutu İle Sütun Silme:

```sql
ALTER TABLE OgrenciBilgileri DROP COLUMN OgrenciAdYeni
```

2. DROP Komutu İle Tablo Silme:

```sql
DROP TABLE OgrenciBilgileri
```

4. DROP Komutu İle Databese Silme:

```sql
DROP DATABASE  Db_Education
```
