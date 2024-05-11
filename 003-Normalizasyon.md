# Normalizasyon

- Normalizasyon, veritabanının tasarım aşamasında veri tekrarını, veri kaybını veya veri yetersizliğini önlemek için gerçekleştirilen işlemlerdir.
- Tabloların oluşturulması, tablolar arasında artıklığı ve tutarsızlığı ortadan kaldırma normalizasyonun temel amacıdır.
- Normalizasyon için gereken birkaç kural vardır. Her bir kurala «Normal Form» ismi verilir.
- İlk kural kullanılıyorsa, veritabanının «birinci normal formda (1NF)» olduğu; ikinci kural kullanılıyorsa veritabanının «ikinci normal formda (2NF)» olduğu, üçüncü kural kullanılıyorsa veritabanının «üçüncü normal formda (3NF)» olduğu söylenir. 4 ve 5. normalizasyon kuralları da vardır ama çoğu uygulama için ilk 3 normalizasyon kuralı yeterlidir.
- Normalizasyon aynı zamanda çok fazla satır ve sütundan oluşan bir tablonun veri tekrarından korumak için daha az satır ve sütun içeren tablolara ayırma işlemidir.
- Üst normalizasyon kuralları, alt normalizasyon kurallarını kapsamaktadır. Yani, 2NF kuralları 1NF kurallarını; 3NF kuralları 1NF ve 2NF kurallarını kapsar.
- Tek bir sütundaki aynı veri tekrar edebilir. Fakat iki veya daha fazla alandan oluşan bir bilginin birden fazla satırda tekrar etmesi istenilmez.

## Normalizasyonun Amaçları

1. Veri Bütünlüğünü Sağlamak: Örneğin bir veritabanı içerisinde öğrenci numarası birden fazla tabloda kullanıldığında veri bütünlüğü sağlanmazsa zamanla aynı öğrenci için birden fazla numara bilgisi oluşabilecektir.
1. Uygulamadan Bağımsızlık: Hazırlanacak ilişkisel model kullanılacak uygulamaya göre değil de içereceği veriye göre hazırlanılırsa, kullanılan uygulama değişse bile tasarım sorunsuz olarak çalışmaya devam edebilir.
1. Performansı Arttırmak: Tam olarak normalleştirilmiş veritabanında veri tekrarı en aza ineceği için kullanılan disk alanı ve veritabanı boyutu da azalacaktır. Böyle bir veritabanında işlemler daha hızlı yapılabilecektir.

## Normalizasyon Kuralları

1. Birinci Normal Form (1NF)

- Her tabloda bir kimlik kolonu olmak zorundadır.
- Her sütun kendi bilgisini içermelidir. Farklı tipte bilgi içeremez. Örnek olarak yıl kolonuna yaş bilgisi girilmemelidir.
- Bir alan içerisindeki bilgi özel karakterlere ayrılarak tutulmamalıdır.
- Birden fazla bilgi tek bir sütunda olamaz. Örnek olarak, Ad Soyad için tek bir sütun oluşturmaktansa Ad için ayrı Soyad için ayrı sütun oluşturulmalıdır.
- Tekrarlanan sütun yapıları olmalıdır.

![Alternatif Metin](Assets/Screenshot11.png)

2. İkinci Normal Form (2NF)

- 1NF kuralının uygulanmış olması beklenir.
- Bir tablo içinde tanımlı ve anahtar olmayan her sütun, birincil anahtar olarak tanımlı anahtar sütuna bağımlı olmalıdır.
- Anahtar sütun, birden fazla sütunun birleşiminden oluşuyorsa tabloda yer alacak veriler iki sütuna da bağımlı olmalıdır. Tek sütuna bağımlı ise ayrı bir tabloda tutulmalıdır.
- Yatay tekrar söz konusu olamaz. Örnek olarak bir öğrenci birden fazla bölümde ise Bölüm1, Bölüm2 gibi kolon tekrarı olmamalıdır.
- Tekrar eden kolondaki veriler farklı tablolara ayrıştırılmalıdır. Bir kolonda veriler tekrar ediyorsa o kolon farklı bir tablo olarak oluşturulmalıdır.

![Alternatif Metin](Assets/Screenshot12.png)

3. Üçüncü Normal Form (3NF)

- 1NF ve 2NF kurallarının uygulanmış olması beklenir.
- Anahtar olmayan sütunlar ya başka bir tablonun birincil anahtarını (PK) referans göstermeli (FK “ikincil anahtar”) ya da aynı tabloda ki diğer alanlar ile doğrudan ilişkili olmalıdır.
- Çoka çok ilişkinin olması durumunda uygulanması gerekir.

![Alternatif Metin](Assets/Screenshot13.png)
![Alternatif Metin](Assets/Screenshot14.png)

4. Boyce-Code Normal Form(BCNF - 3.5NF)

Eğer bir tablo 3NF kuralına uygunsa ve her bir belirleyici aday anahtar ise tablo BCNF kuralına uygundur.

5. Dördüncü Normal Form (4NF)

- Veri Tabanı 3NF olmalıdır,
- Çok-değerli bağımlılıkları (Multli-Valued dependency) olmamalıdır.

Çok-Değerli Bağımlılık (Multli-Valued Dependency): Şayet herhangi bir kolon (örneğin X olsun), aynı kalmak şartıyla, ikinci bir kolondaki (örneğin Y olsun) veriler değişiyorsa, bu durumda çok değerli bağımlılık vardır diyebiliriz.
