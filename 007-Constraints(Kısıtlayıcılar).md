
# Contraints (Kısıtlayıcılar)

Constraints(Kısıtlayıcılar) aracılığı ile tablolar üzerinde istediğimiz şartlar ve durumlara göre kısıtlamalar yapılabilir.

1. Default Constraint
2. Check Constraint
3. Primary Key Constraint
4. Unique Constraint
5. Foreign Key Constraint

## Default Constraint

Bir kolona bir değer girilmediği taktirde varsayılan olarak ne girilmesi gerektiğini belirlediğimiz kısıtlamadır.

```sql
CREATE TABLE OrnekTablo
(
 UrunKimligi INT NOT NULL,
 UrunAdi  NVARCHAR(30),
 SatisMiktari INT NOT NULL,
 KarMiktari INT
)

-- Örnek 1
ALTER TABLE OrnekTablo
ADD CONSTRAINT UrunAdi_DefaultConstraint DEFAULT 'Ürün tanımlı deil' FOR UrunAdi

-- Örnek 2
ALTER TABLE OrnekTablo
ADD CONSTRAINT SatisMiktari_DefaultConstraint DEFAULT 0 FOR SatisMiktari
```

## Check Constraint

Bir kolona girilecek olan verinin belirli bir şarta uymasını zorunlu tutan kısıtlamadır.

```sql
CREATE TABLE OrnekTablo
(
 UrunKimligi INT NOT NULL,
 UrunAdi  NVARCHAR(30),
 SatisMiktari INT NOT NULL,
 KarMiktari INT
)

ALTER TABLE OrnekTablo
ADD CONSTRAINT KarMiktari_CheckConstraint CHECK ((KarMiktari * 5) % 2 = 0)
```

**Not:** Check Constraint oluşturulmadan önce tabloda şarta aykırı değerler var ise constraint oluşurken hata verir. Önceki kayıtları görmezden gelip oluşturmak istiyorsak "WİTH NOCHECK" komutu kullanılmalıdır.

```sql
ALTER TABLE OrnekTablo
WITH NOCHECK ADD CONSTRAINT KarMiktari_CheckConstraint CHECK ((KarMiktari * 5) % 2 = 0)
```

## Primary Key Constraint

Birincil anahtar, tablodaki her bir satırın benzersiz olarak tanımlanmasına izin veren bir sütun veya sütun kombinasyonudur. Bir kolona primary key(birincil anahtar) özelliğini eklemek için  primary key constraint kullanılır. Bu sayede o kolona eklenen primary key ile başka tablolarda foreign key oluşturarak ilişki kurmamız mümkün olur. Bunun yanında o kolonun taşıdığı verilerin tekil olacağıda(unique) garanti edilmiş olur.

Kurallar:
1- Primary key constraint kullanılan kolon primary key özelliğe sahip olmamalıdır.
2- Primary key constraint özelliği kullanılacağı zaman o tabloda başka primary key özelliğe sahip kolon olmamalıdır. 
3- Bir tabloda yalnızca bir primary key olabilir. Ancak, bir primary key birden fazla sütunu içerebilir, bu durumda birden fazla sütunun birleşimi tablodaki her bir satırı benzersiz bir şekilde tanımlar.

```sql
CREATE TABLE OrnekTablo
(
 UrunKimligi INT NOT NULL,
 UrunAdi  NVARCHAR(30),
 SatisMiktari INT NOT NULL,
 KarMiktari INT
)

ALTER TABLE OrnekTablo
ADD CONSTRAINT UrunKimligi_PrimaryKeyConstraint PRIMARY KEY (UrunKimligi)

ALTER TABLE OrnekTablo
ADD CONSTRAINT UrunKimligi_PrimaryKeyConstraint PRIMARY KEY (UrunKimligi, UrunAdi)
```

## Unique Constraint

Tek amacı belirttiğimiz kolondaki verilerin tekil olmasını sağlamaktır. 

```sql
CREATE TABLE OrnekTablo
(
 UrunKimligi INT NOT NULL,
 UrunAdi  NVARCHAR(30),
 SatisMiktari INT NOT NULL,
 KarMiktari INT
)

ALTER TABLE OrnekTablo
ADD CONSTRAINT SatisMiktari_UniqueConstraint UNIQUE (SatisMiktari)
```

## Foreign Key Constraint

Tabloların kolonları arasında ilişki kurmamızı sağlar. Bu ilişki neticesinde, foreign key özelliğine sahip kolonda ki karşılığının boşa düşmemesi için primary key özelliğine sahip kolonun bulunduğu tablodan veri silinmesini ve güncellemesini engeller.

```sql
CREATE TABLE Erkekler
(
 BayID INT PRIMARY KEY NOT NULL IDENTITY(1,1),  
 BayanID INT,
 BayAdSoyad NVARCHAR(MAX),
)

CREATE TABLE Bayanlar
(
BayanID INT PRIMARY KEY NOT NULL IDENTITY(1,1),
BayanAdSoyad NVARCHAR(MAX)
)

ALTER TABLE Erkekler
ADD CONSTRAINT BayanID_ForeignKeyConstraint FOREIGN KEY (BayanID) REFERENCES Bayanlar(BayanID)
```
## Identity Kullanımı

Belirlenen kolonun otomatik olarak artması için kullanılır. Identity özelliği her tablo için bir kez kullanılabilir. 

Kullanım Prototipi: IDENTITY([Başlangıç parametresi], [artış parametresi])

```sql
CREATE TABLE Erkekler
(
 BayID INT PRIMARY KEY NOT NULL IDENTITY(1,1),  
 BayanID INT,
 BayAdSoyad NVARCHAR(MAX),
)
```











