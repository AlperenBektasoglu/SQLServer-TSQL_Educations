
# Temel Sql Komutları

## Top Komutu
SELECT TOP komutu, çağırılacak verilerin sayısını belirtmek için kullanılır. Tüm veritabanı sistemleri SELECT TOP komutunu desteklemez. MySQL sınırlı sayıda veri seçmek için LIMIT komutunu desteklerken, Oracle ROWNUM kullanır.

```sql
USE Northwind
SELECT TOP 3 * FROM Personeller

USE Db_Education
UPDATE TOP(2) DenemePersoneller SET Isım = 'Boş' 
```

## Distinct Komutu

Bir kolondaki benzer olan verileri teke indirmemizi sağlayan komuttur. 

```sql
USE Northwind
SELECT Sehir FROM Personeller
SELECT DISTINCT Sehir FROM personeller
SELECT COUNT(DISTINCT Sehir) FROM personeller
```

## Group By Komutu




