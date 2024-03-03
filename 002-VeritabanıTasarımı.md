# VeritabanıTasarımı

## Veritabanı Tasarım Aşamaları
![Alternatif Metin](Assets/Screenshot1.png)

## Entity Relationship Model (ER)
![Alternatif Metin](Assets/Screenshot2.png)
![Alternatif Metin](Assets/Screenshot3.png)
![Alternatif Metin](Assets/Screenshot4.png)
![Alternatif Metin](Assets/Screenshot5.png)
![Alternatif Metin](Assets/Screenshot9.png)

### Recursive İlişki
İlişkiler çoğunlukla farklı varlık kümeleri ile oluşturulur. Ancak bazı durumlarda tek bir varlık kümesi kendi ile ilişki kurabilir. 
Örneğin bir kişi hem personel hem yönetici olabilir. Yani birden fazla kişi bir kişinin yönetiminde olabilir ve bir kişi birden fazla kişiyi yönetir. 

![Alternatif Metin](Assets/Screenshot10.png)

### İlişki Türleri
* 1-1 İlişki (Bire-Bir)
İki kayıt kümesinden birindeki bir kaydın diğer tarafta sadece bir kayıtla eşleşmesi durumudur. 
Örneğin bir erkeğin tek bir kadınla evli olması gibi. Tablolar oluşturulurken bir tablonun birincil anahtarı,
diğer tablonun yabancıl anahtarı olarak belirlenir.

![Alternatif Metin](Assets/Screenshot6.png)

* 1-n (Bir-Çok/Çok-Bir)
İki kayıt kümesinden birindeki bir kaydın diğer tarafta birçok kayıtla eşleşmesi durumudur. 
Örneğin bir hastanın defalarca muayene olabilmesi. Tablolar oluşturulurken ilişkinin n tarafındaki tabloya, 1 tarafındaki
tablonun birincil anahtar sütunu yabancıl anahtar olarak eklenir.

![Alternatif Metin](Assets/Screenshot7.png)

* Çoka-Çok (n-m)
İki kayıt kümesinden her iki taraftaki kayıtların diğer tarafta birçok kayıtla eşleşmesi durumudur. 
Örneğin bir filmin aynı anda farklı sinemalarda oynarken bir yandan aynı sinemada başka filmlerin de oynatılabilmesi.
Tablolar oluşturulurken ilişki adında yeni bir tablo oluşturulur ve diğer tabloların birincil anahtarı yeni oluşan
tabloya yabancıl anahtar olarak eklenir. İlişkiden doğan nitelikler de ilişki tablosuna kolon olarak eklenir.

![Alternatif Metin](Assets/Screenshot8.png)




