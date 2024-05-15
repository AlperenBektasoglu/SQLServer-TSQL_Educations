# SQLServer-TSQL_Educations

![Türkiye](https://raw.githubusercontent.com/stevenrskelton/flag-icon/master/png/16/country-4x3/tr.png "Türkiye") TR: SQL Server ve T-SQL Eğitimleri

![United Kingdom](https://raw.githubusercontent.com/stevenrskelton/flag-icon/master/png/16/country-4x3/gb.png "United Kingdom") EN: SQL Server And T-SQL Educations

## Table Of Contents

- [Genel Bilgiler](001-GenelBilgiler.md#genel-bilgiler)

  - [Veri/Data Nedir?](001-GenelBilgiler.md#veri/data-nedir)
  - [Veritabanı/Database Nedir?](001-GenelBilgiler.md#veritabanı/database-nedir)
  - [Veritabanı Yönetim Sistemi (VTYS)/Database Management System(DBMS) Nedir?](001-GenelBilgiler.md#veritabanı-yönetim-sistemi-vtys/database-management-systemdbms-nedir)
  - [VTYS’lerin Faydaları](001-GenelBilgiler.md#vtys’lerin-faydaları)
  - [Veri Modelleri/Data Model](001-GenelBilgiler.md#veri-modelleri/data-model)
    - [İlişkisel Veri Modeli (Relational Data Model)](001-GenelBilgiler.md#i̇lişkisel-veri-modeli-relational-data-model)

- [Veritabanı Tasarımı](002-VeritabanıTasarımı.md#veritabanı-tasarımı)

  - [Veritabanı Tasarım Aşamaları](002-VeritabanıTasarımı.md#veritabanı-tasarım-aşamaları)
  - [ORM (Object Role Modelling)](002-VeritabanıTasarımı.md#orm-object-role-modelling)
  - [UML (Unified Modeling Language)](002-VeritabanıTasarımı.md#uml-unified-modeling-language)
  - [Entity Relationship Model (ER)](002-VeritabanıTasarımı.md#entity-relationship-model-er)
    - [Recursive İlişki](002-VeritabanıTasarımı.md#recursive-i̇lişki)
    - [İlişki Türleri](002-VeritabanıTasarımı.md#i̇lişki-türleri)

- [Normalizasyon](003-Normalizasyon.md#normalizasyon)

  - [Normalizasyonun Amaçları](003-Normalizasyon.md#normalizasyonun-amaçları)
  - [Normalizasyon Kuralları](003-Normalizasyon.md#normalizasyon-kuralları)

- [Sql Temel Kavramlar](004-SqlTemelKavramlar.md#sql-temel-kavramlar)

  - [Sql Server Genel İsimlendirme Kuralları](004-SqlTemelKavramlar.md#sql-server-genel-i̇simlendirme-kuralları)
  - [Genel Bilgiler](004-SqlTemelKavramlar.md#genel-bilgiler)
  - [Use Anahtar Kelimesi](004-SqlTemelKavramlar.md#use-anahtar-kelimesi)
  - [Sql Komut Kategorileri](004-SqlTemelKavramlar.md#sql-komut-kategorileri)
  - [Sql Dosya Formatları](004-SqlTemelKavramlar.md#sql-dosya-formatları)
  - [Sıklıkla Kullanılan Veri Tipleri](004-SqlTemelKavramlar.md#sıklıkla-kullanılan-veri-tipleri)
    - [Unicode Nedir? Unicode ile ASCII Farkları Nelerdir?](004-SqlTemelKavramlar.md#unicode-nedir-unicode-ile-ascii-farkları-nelerdir)
  - [Uniqueidentifier Veri Tipi](004-SqlTemelKavramlar.md#uniqueidentifier-veri-tipi)

- [Operatörler](005-Operatörler.md#operatörler)

  - [Karşılaştırma Operatörleri](005-Operatörler.md#karşılaştırma-operatörleri)
  - [Mantıksal Operatörler](005-Operatörler.md#mantıksal-operatörler)
  - [Aritmetiksel Operatörler](005-Operatörler.md#aritmetiksel-operatörler)

- [DDL Komutları](006-DDLKomutları.md#ddl-komutları)

  - [DDL(Data Definition Language) Komutları (CREATE / ALTER / DROP)](006-DDLKomutları.md#ddldata-definition-language-komutları-create-/-alter-/-drop)
  - [CREATE Komutu](006-DDLKomutları.md#create-komutu)
  - [ALTER Komutu](006-DDLKomutları.md#alter-komutu)
  - [DROP Komutu](006-DDLKomutları.md#drop-komutu)

- [Contraints (Kısıtlayıcılar)](<007-Constraints(Kısıtlayıcılar).md#contraints-kısıtlayıcılar>)

  - [Default Constraint](<007-Constraints(Kısıtlayıcılar).md#default-constraint>)
  - [Check Constraint](<007-Constraints(Kısıtlayıcılar).md#check-constraint>)
  - [Primary Key Constraint](<007-Constraints(Kısıtlayıcılar).md#primary-key-constraint>)
  - [Unique Constraint](<007-Constraints(Kısıtlayıcılar).md#unique-constraint>)
  - [Foreign Key Constraint](<007-Constraints(Kısıtlayıcılar).md#foreign-key-constraint>)
  - [Identity Kullanımı](<007-Constraints(Kısıtlayıcılar).md#identity-kullanımı>)
  - [Constraint Nasıl Silinir?](<007-Constraints(Kısıtlayıcılar).md#constraint-nasıl-silinir>)
  - [Cascade Komutu](<007-Constraints(Kısıtlayıcılar).md#cascade-komutu>)
  - [Set Default Komutu](<007-Constraints(Kısıtlayıcılar).md#set-default-komutu>)
  - [Create Komutu içerisinde Constraint'lerin Kullanılması](<007-Constraints(Kısıtlayıcılar).md#create-komutu-içerisinde-constraint'lerin-kullanılması>)

- [Select Kullanımı](008-SelectKullanımı.md#select-kullanımı)

  - [Select Komutu](008-SelectKullanımı.md#select-komutu)
  - [Alias(Takma Ad) Kulanımı](008-SelectKullanımı.md#aliastakma-ad-kulanımı)
  - [Kolon Birleştirme](008-SelectKullanımı.md#kolon-birleştirme)
  - [Tablo Oluştururken As komutu kullanma](008-SelectKullanımı.md#tablo-oluştururken-as-komutu-kullanma)
  - [Select Komutunda Where Şartı](008-SelectKullanımı.md#select-komutunda-where-şartı)
    - [Mantıksal Operatörlerin Kullanımı](008-SelectKullanımı.md#mantıksal-operatörlerin-kullanımı)
    - [Between Kullanımı](008-SelectKullanımı.md#between-kullanımı)
    - [In / Not In Kullanımı](008-SelectKullanımı.md#in-/-not-in-kullanımı)
    - [Where Bloğunda Kullanılan Bağlaçların İşlem Önceliği Sıralaması](008-SelectKullanımı.md#where-bloğunda-kullanılan-bağlaçların-i̇şlem-önceliği-sıralaması)
  - [Select Komutunda Like Şartı](008-SelectKullanımı.md#select-komutunda-like-şartı)
    - [Like Sorgusunda Escape(Kaçış) Karakteri](008-SelectKullanımı.md#like-sorgusunda-escapekaçış-karakteri)

- [DML Komutları](009-DMLKomutları.md#dml-komutları)

  - [DML(Data Manipulation Language) Komutları (INSERT/ UPDATE/ DELETE)](009-DMLKomutları.md#dmldata-manipulation-language-komutları-insert/-update/-delete)
  - [Insert Komutu](009-DMLKomutları.md#insert-komutu)
    - [Insert Komutu İle Select Sorgusu Sonucunda Gelen Verileri Farklı Bir Tabloya Kaydetme](009-DMLKomutları.md#insert-komutu-i̇le-select-sorgusu-sonucunda-gelen-verileri-farklı-bir-tabloya-kaydetme)
  - [Update Komutu](009-DMLKomutları.md#update-komutu)
  - [Delete Komutu](009-DMLKomutları.md#delete-komutu)
  - [Truncate Komutu](009-DMLKomutları.md#truncate-komutu)

- [Özel Fonksiyonlar](010-ÖzelFonksiyonlar.md#özel-fonksiyonlar)

  - [Aggregate Fonksiyonları](010-ÖzelFonksiyonlar.md#aggregate-fonksiyonları)
  - [String Fonksiyonları](010-ÖzelFonksiyonlar.md#string-fonksiyonları)
  - [Sayısal Değer İşlemleri ve Sayısal Fonksiyonlar](010-ÖzelFonksiyonlar.md#sayısal-değer-i̇şlemleri-ve-sayısal-fonksiyonlar)
  - [Tarih Fonksiyonları](010-ÖzelFonksiyonlar.md#tarih-fonksiyonları)
  - [Coalesce Fonksiyonu İle Null Değer Kontrolü](010-ÖzelFonksiyonlar.md#coalesce-fonksiyonu-i̇le-null-değer-kontrolü)
  - [IsNull Fonksiyonu İle Null Değer Kontrolü](010-ÖzelFonksiyonlar.md#isnull-fonksiyonu-i̇le-null-değer-kontrolü)
  - [NullIf Fonksiyonu İle Null Değer Kontrolü](010-ÖzelFonksiyonlar.md#nullif-fonksiyonu-i̇le-null-değer-kontrolü)

- [Temel Sql Komutları](011-TemelSqlKomutları.md#temel-sql-komutları)

  - [Top Komutu](011-TemelSqlKomutları.md#top-komutu)
    - [Top Komutu İle Update İşlemi](011-TemelSqlKomutları.md#top-komutu-i̇le-update-i̇şlemi)
    - [Top Komutu İle Delete İşlemi](011-TemelSqlKomutları.md#top-komutu-i̇le-delete-i̇şlemi)
  - [Distinct Komutu](011-TemelSqlKomutları.md#distinct-komutu)
  - [Group By Komutu](011-TemelSqlKomutları.md#group-by-komutu)
  - [Having Komutu](011-TemelSqlKomutları.md#having-komutu)
  - [With Rollup Komutu](011-TemelSqlKomutları.md#with-rollup-komutu)
  - [With Cube Komutu](011-TemelSqlKomutları.md#with-cube-komutu)
  - [Order By Komutu](011-TemelSqlKomutları.md#order-by-komutu)
  - [With Ties komutu](011-TemelSqlKomutları.md#with-ties-komutu)
  - [Toplu Kullanım](011-TemelSqlKomutları.md#toplu-kullanım)
  - [Partition By Kullanımı](011-TemelSqlKomutları.md#partition-by-kullanımı)

- [Joinler](012-Joinler.md#joinler)

  - [Inner Join Yapısı](012-Joinler.md#inner-join-yapısı)
    - [Inner Join İle Birden Fazla Tabloyu Birleştirme](012-Joinler.md#inner-join-i̇le-birden-fazla-tabloyu-birleştirme)
    - [Recursive İlişki](012-Joinler.md#recursive-i̇lişki)
  - [Left Outher Join Yapısı](012-Joinler.md#left-outher-join-yapısı)
  - [Right Outher Join Yapısı](012-Joinler.md#right-outher-join-yapısı)
  - [Full Outher Join Yapısı](012-Joinler.md#full-outher-join-yapısı)
  - [Cross Join Yapısı](012-Joinler.md#cross-join-yapısı)
  - [Join Yapısında Group By Komutu Kullanma](012-Joinler.md#join-yapısında-group-by-komutu-kullanma)
  - [Intersect Komutu](012-Joinler.md#intersect-komutu)
  - [Except Komutu](012-Joinler.md#except-komutu)
  - [Union Komutu](012-Joinler.md#union-komutu)
  - [Union All Komutu](012-Joinler.md#union-all-komutu)

- [Alt Sorgular](013-AltSorgular.md#alt-sorgular)

  - [With Kullanımı](013-AltSorgular.md#with-kullanımı)

- [Views (Sanal Tablolar)](014-Views.md#views-sanal-tablolar)

  - [View Oluşturma](014-Views.md#view-oluşturma)
  - [View'in Yapısını Değiştirme](014-Views.md#view'in-yapısını-değiştirme)
  - [Varolan View’in Kodunu Görme](014-Views.md#varolan-view’in-kodunu-görme)
  - [Tanımlı View Listesini Görme](014-Views.md#tanımlı-view-listesini-görme)
  - [View Kodunun Şifrelenmesi / With Encryption Komutu](014-Views.md#view-kodunun-şifrelenmesi-/-with-encryption-komutu)
  - [View’in Silinmesi](014-Views.md#view’in-silinmesi)
  - [View’daki Tabloya Kayıt Ekleme / Güncelleme / Silme](014-Views.md#view’daki-tabloya-kayıt-ekleme-/-güncelleme-/-silme)
  - [With Check Option Komutu](014-Views.md#with-check-option-komutu)
  - [View’in Adını Değiştirme](014-Views.md#view’in-adını-değiştirme)

- [Programlama Yapıları](015-ProgramlamaYapıları.md#programlama-yapıları)

  - [Sql'de Değişken Tanımlama](015-ProgramlamaYapıları.md#sql'de-değişken-tanımlama)
  - [Birleşik Operatörler](015-ProgramlamaYapıları.md#birleşik-operatörler)
  - [Go Komutu](015-ProgramlamaYapıları.md#go-komutu)
  - [If-Else Yapısı](015-ProgramlamaYapıları.md#if-else-yapısı)
  - [While Döngüsü](015-ProgramlamaYapıları.md#while-döngüsü)
  - [Break Komutu](015-ProgramlamaYapıları.md#break-komutu)
  - [Continue Komutu](015-ProgramlamaYapıları.md#continue-komutu)
  - [Case Kullanımı](015-ProgramlamaYapıları.md#case-kullanımı)
  - [Exists / Not Exists Fonksiyonu](015-ProgramlamaYapıları.md#exists-/-not-exists-fonksiyonu)
  - [Tablo Tipi Değişkenler](015-ProgramlamaYapıları.md#tablo-tipi-değişkenler)
  - [Try-Catch Yapısı](015-ProgramlamaYapıları.md#try-catch-yapısı)

- [Stored Procedures (SP)](016-StoredProcedures.md#stored-procedures-sp)

  - [Stored Procedure Oluşturma](016-StoredProcedures.md#stored-procedure-oluşturma)
  - [Alter Komutu İle Stored Procedure Yapısını Değiştirme](016-StoredProcedures.md#alter-komutu-i̇le-stored-procedure-yapısını-değiştirme)
  - [Stored Procedure İle With Encryption Komutunun Kullanımı](016-StoredProcedures.md#stored-procedure-i̇le-with-encryption-komutunun-kullanımı)
  - [Stored Procedure İle With Recompile Komutunun Kullanımı](016-StoredProcedures.md#stored-procedure-i̇le-with-recompile-komutunun-kullanımı)
  - [Stored Procedure İle Set Nocount On Komutunun Kullanımı](016-StoredProcedures.md#stored-procedure-i̇le-set-nocount-on-komutunun-kullanımı)
  - [Stored Procedure Silme](016-StoredProcedures.md#stored-procedure-silme)

- [Fonksiyonlar](017-Fonksiyonlar.md#fonksiyonlar)

  - [Fonksiyon ile Store Procedure Arasındaki Farklar](017-Fonksiyonlar.md#fonksiyon-ile-store-procedure-arasındaki-farklar)
  - [Fonksiyon Tipleri](017-Fonksiyonlar.md#fonksiyon-tipleri)
  - [Scalar Fonksiyonlar](017-Fonksiyonlar.md#scalar-fonksiyonlar)
  - [Inline Fonksiyonlar](017-Fonksiyonlar.md#inline-fonksiyonlar)
  - [Alter Komutu İle Fonksiyon Yapısını Değiştirme](017-Fonksiyonlar.md#alter-komutu-i̇le-fonksiyon-yapısını-değiştirme)
  - [Fonsiyon Silme](017-Fonksiyonlar.md#fonsiyon-silme)

- [Triggers(Tetikleyiciler)](018-Triggers.md#triggerstetikleyiciler)

  - [DDL Trigger](018-Triggers.md#ddl-trigger)
  - [DML Trigger](018-Triggers.md#dml-trigger)
  - [Trigger'ı Devre Dışı Bırakma - Aktifleştirme](018-Triggers.md#trigger'ı-devre-dışı-bırakma---aktifleştirme)
  - [Alter Komutu İle DDL Trigger Güncelleme](018-Triggers.md#alter-komutu-i̇le-ddl-trigger-güncelleme)
  - [Alter Komutu İle DML Trigger Güncelleme](018-Triggers.md#alter-komutu-i̇le-dml-trigger-güncelleme)
  - [Trigger Silme](018-Triggers.md#trigger-silme)

- [Transaction Yapısı](019-TransactionYapısı.md#transaction-yapısı)

  - [WAITFOR Kullanımı](019-TransactionYapısı.md#waitfor-kullanımı)

- [Indexes](020-Indexes.md#indexes)

  - [Index Nedir?](020-Indexes.md#index-nedir)
  - [Indexlerin Çalışma Prensibi](020-Indexes.md#indexlerin-çalışma-prensibi)
  - [B-tree(Balance Tree) Yapısının Çalışma Mantığı](020-Indexes.md#b-treebalance-tree-yapısının-çalışma-mantığı)
  - [Heap Table](020-Indexes.md#heap-table)
  - [Index Çeşitleri](020-Indexes.md#index-çeşitleri)
    - [Clustered Index](020-Indexes.md#clustered-index)
      - [Clustered Index Oluşturma](020-Indexes.md#clustered-index-oluşturma)
    - [Non-Clustered Index](020-Indexes.md#non-clustered-index)
      - [Non-Clustered Index Oluşturma](020-Indexes.md#non-clustered-index-oluşturma)
  - [Index Tanımlama Yaklaşımları](020-Indexes.md#index-tanımlama-yaklaşımları)
  - [Composite Index Oluşturma](020-Indexes.md#composite-index-oluşturma)
  - [Unique Index Oluşturma](020-Indexes.md#unique-index-oluşturma)
  - [Komplex Index Örneği](020-Indexes.md#komplex-index-örneği)
  - [Index Silme](020-Indexes.md#index-silme)

- [Cursors](021-Cursors.md#cursors)

  - [Cursor Nedir?](021-Cursors.md#cursor-nedir)
  - [Cursor Kaydırma Komutları](021-Cursors.md#cursor-kaydırma-komutları)
  - [Cursor Türleri Nelerdir?](021-Cursors.md#cursor-türleri-nelerdir)
    - [Statik Cursorlar](021-Cursors.md#statik-cursorlar)
    - [Keyset-Driven Cursorlar](021-Cursors.md#keyset-driven-cursorlar)
    - [Dinamik Cursorlar](021-Cursors.md#dinamik-cursorlar)
  - [Cursor’da Sütun Güncelleme(FOR UPDATE)](021-Cursors.md#cursor’da-sütun-güncellemefor-update)

- [Geçici Tablolar - Temporary Tables](022-GeçiciTablolar.md#geçici-tablolar---temporary-tables)

- [Backup / Restore](023-BackupRestore.md#backup-/-restore)

  - [Veritabanının Recovery Modelinin Öğrenilmesi](023-BackupRestore.md#veritabanının-recovery-modelinin-öğrenilmesi)
  - [Yedekleme Türleri](023-BackupRestore.md#yedekleme-türleri)
    - [Full Backup (Tam Yedekleme)](023-BackupRestore.md#full-backup-tam-yedekleme)
    - [Differential Backup (Fark Yedekleme)](023-BackupRestore.md#differential-backup-fark-yedekleme)
    - [Transaction Log Backup (İşlem Günlüğü Yedekleme)](023-BackupRestore.md#transaction-log-backup-i̇şlem-günlüğü-yedekleme)
  - [Full Recovery Model’de Full Backup](023-BackupRestore.md#full-recovery-model’de-full-backup)
  - [Full Model’de Restore](023-BackupRestore.md#full-model’de-restore)
  - [Farklı Backup Türlerinin Dosya Boyutları](023-BackupRestore.md#farklı-backup-türlerinin-dosya-boyutları)
  - [Backup-Restore’un SQL Kodlarıyla Yapılması](023-BackupRestore.md#backup-restore’un-sql-kodlarıyla-yapılması)
  - [BackUp İşleminin Job Olarak Yapılması](023-BackupRestore.md#backup-i̇şleminin-job-olarak-yapılması)

- [Diğer Konular](024-DiğerKonular.md#diğer-konular)
  - [IDENTITY Kullanımı](024-DiğerKonular.md#identity-kullanımı)
  - [ROWCOUNT Kullanımı](024-DiğerKonular.md#rowcount-kullanımı)
  - [Veritabanındaki Tabloları Listeleme](024-DiğerKonular.md#veritabanındaki-tabloları-listeleme)
  - [Bir Tabloda Primary Key Olup Olmadığını Kontrol Etme](024-DiğerKonular.md#bir-tabloda-primary-key-olup-olmadığını-kontrol-etme)
  - [En Son Primary Key Id Değerinin Bulunması](024-DiğerKonular.md#en-son-primary-key-id-değerinin-bulunması)
  - [Default Values Kullanımı](024-DiğerKonular.md#default-values-kullanımı)
  - [ROW_NUMBER() Fonksiyonu](024-DiğerKonular.md#row_number-fonksiyonu)
  - [Ansi_Nulls Komutu](024-DiğerKonular.md#ansi_nulls-komutu)
  - [Dynamic Data Masking](024-DiğerKonular.md#dynamic-data-masking)
  - [Temporal Tables](024-DiğerKonular.md#temporal-tables)
  - [Row Level Security](024-DiğerKonular.md#row-level-security)
  - [Execution Plan Nedir?](024-DiğerKonular.md#execution-plan-nedir)
  - [Sql Bulk Insert](024-DiğerKonular.md#sql-bulk-insert)
  - [DCL (Data Control Language - Veri Kontrol Dili)](024-DiğerKonular.md#dcl-data-control-language---veri-kontrol-dili)
