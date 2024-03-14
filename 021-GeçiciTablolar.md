# Geçici Tablolar - Temporary Tables
Genellikle bir Sql Server üzerinde farklı lokasyonlarda birden fazla kişinin çalıştığı durumlarda ya da verilerin test amaçlı geçici bir yerde tutulması, işlenmesi amacıyla kullanılan yapılardır. Bilinen tablo yapısının aynısını sağlar. Tek farkı fiziksel olarak oluşmazlar. Sadece bellekte geçici olarak oluşturulurlar. SELECT, INSERT, UPDATE ve DELETE işlemleri yapılabilir. İlişki kurulabilir. Sunucu kapatıldığında ya da oturum sahibi oturumu kapattığında bellekten silinirler.

```sql
-- Bir tabloyu geçici tabloya kopyalama
SELECT * INTO #GeciciTbl FROM Personeller

-- SELECT * FROM #GeciciTbl
-- INSERT #GeciciTbl (Adi, Soyadi) VALUES ('Alperen', 'Bektaşoğlu')
```

**Not:** Geçici tablolar # ile başlayacağı gibi ## ile de başlayabilir. # ile başlayan geçici tabloya sadece oturum sahibi erişebilirken, ## ile başlayan tabloya oturum açan şahıs ve onun Sql Serverına dışarıdan ulaşan 3. şahıslar kullanabilir.


