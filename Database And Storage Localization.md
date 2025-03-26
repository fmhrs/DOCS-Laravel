# Database
## MYSQL

- ubah `DB_CONNECTION` pada file `.env` menjadi `mysql` lalu uncomment `DB_HOST` hingga `DB_PASSWORD` seperti berikut
- minus: karena menggunakan `mysql` harus menyalakan `mysql` entah menggunakan xampp atau secara langsung setiap run server, 
```c
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=tokoadif
DB_USERNAME=root
DB_PASSWORD=
```

## SQLITE
- buat file `database/database.sqlite` atau bisa menggunakan terminal `touch database/database.sqlite`
- atur `.env` sebagai berikut (nb: db_database tidak perlu di atur bila file dibuat persis seperti langkah diatas)
```c
DB_CONNECTION=sqlite
DB_DATABASE=/absolute/path/to/database.sqlite // tidak perlu di atur bila nama dan lokasi adalah database/database.sqlite
```
- jika error saat pertama kali membuka halaman coba jalankan `php artisan migrate:fresh`



### **📌 Tabel: Ukuran dan Range Tipe Data Laravel**

|**Tipe Data**|**Ukuran (Bytes)**|**Range / Keterangan**|
|---|---|---|
|**🔹 Boolean Types**|||
|`boolean()`|1 Byte|`true` / `false` (Disimpan sebagai `TINYINT(1)`)|
|**🔹 String & Text Types**|||
|`char(length)`|1 Byte per char|Maksimal 255 karakter|
|`string(length)`|1 Byte per char|Default 255 karakter, maksimal 65,535 karakter|
|`text()`|64 KB|Maksimal 65,535 karakter|
|`tinyText()`|255 Bytes|Maksimal 255 karakter|
|`mediumText()`|16 MB|Maksimal 16,777,215 karakter|
|`longText()`|4 GB|Maksimal 4,294,967,295 karakter|
|**🔹 Numeric Types**|||
|`bigIncrements()`|8 Bytes|`0` – `18,446,744,073,709,551,615` (Auto Increment)|
|`bigInteger()`|8 Bytes|`-9,223,372,036,854,775,808` – `9,223,372,036,854,775,807`|
|`decimal(total, places)`|Bervariasi|Contoh: `decimal(10,2)` menyimpan `99999999.99`|
|`double(total, places)`|8 Bytes|Nilai desimal dengan presisi tinggi|
|`float(total, places)`|4 Bytes|Nilai desimal dengan presisi lebih rendah dari `double`|
|`id()`|8 Bytes|Alias dari `bigIncrements()`|
|`increments()`|4 Bytes|`0` – `4,294,967,295` (Auto Increment)|
|`integer()`|4 Bytes|`-2,147,483,648` – `2,147,483,647`|
|`mediumIncrements()`|3 Bytes|`0` – `16,777,215` (Auto Increment)|
|`mediumInteger()`|3 Bytes|`-8,388,608` – `8,388,607`|
|`smallIncrements()`|2 Bytes|`0` – `65,535` (Auto Increment)|
|`smallInteger()`|2 Bytes|`-32,768` – `32,767`|
|`tinyIncrements()`|1 Byte|`0` – `255` (Auto Increment)|
|`tinyInteger()`|1 Byte|`-128` – `127`|
|`unsignedBigInteger()`|8 Bytes|`0` – `18,446,744,073,709,551,615`|
|`unsignedInteger()`|4 Bytes|`0` – `4,294,967,295`|
|`unsignedMediumInteger()`|3 Bytes|`0` – `16,777,215`|
|`unsignedSmallInteger()`|2 Bytes|`0` – `65,535`|
|`unsignedTinyInteger()`|1 Byte|`0` – `255`|
|**🔹 Date & Time Types**|||
|`dateTime()`|8 Bytes|`1000-01-01` – `9999-12-31 23:59:59`|
|`dateTimeTz()`|8 Bytes|Sama dengan `dateTime()` dengan zona waktu|
|`date()`|3 Bytes|`1000-01-01` – `9999-12-31`|
|`time()`|3 Bytes|`-838:59:59` – `838:59:59`|
|`timeTz()`|3 Bytes|Sama dengan `time()`, dengan zona waktu|
|`timestamp()`|4 Bytes|`1970-01-01 00:00:01` – `2038-01-19 03:14:07` (Batas 32-bit)|
|`timestamps()`|4 Bytes|Menambahkan `created_at` & `updated_at` otomatis|
|`timestampsTz()`|4 Bytes|Sama dengan `timestamps()`, dengan zona waktu|
|`softDeletes()`|4 Bytes|Menambahkan `deleted_at` untuk soft delete|
|`softDeletesTz()`|4 Bytes|Sama dengan `softDeletes()`, dengan zona waktu|
|`year()`|1 Byte|`1901` – `2155`|
|**🔹 Binary Types**|||
|`binary()`|Maks 2 GB|Data biner (misalnya gambar atau file)|
|**🔹 Object & JSON Types**|||
|`json()`|Maks 4 GB|Format JSON|
|`jsonb()`|Maks 4 GB|Format JSON lebih efisien|
|**🔹 UUID & ULID Types**|||
|`ulid()`|16 Bytes|Universally Unique Lexicographically Sortable Identifier|
|`ulidMorphs()`|16 Bytes|ULID untuk relasi morphs|
|`uuid()`|16 Bytes|Universally Unique Identifier|
|`uuidMorphs()`|16 Bytes|UUID untuk relasi morphs|
|`nullableUlidMorphs()`|16 Bytes|ULID yang bisa null|
|`nullableUuidMorphs()`|16 Bytes|UUID yang bisa null|
|**🔹 Spatial Types**|||
|`geography()`|Bervariasi|Data geografis|
|`geometry()`|Bervariasi|Data spasial (misalnya koordinat)|
|**🔹 Relationship Types**|||
|`foreignId()`|8 Bytes|Alias untuk `unsignedBigInteger()`, digunakan untuk foreign key|
|`foreignIdFor(Model::class)`|8 Bytes|Foreign key berdasarkan model|
|`foreignUlid()`|16 Bytes|Foreign key dengan ULID|
|`foreignUuid()`|16 Bytes|Foreign key dengan UUID|
|`morphs()`|8 + 16 Bytes|Untuk relasi polimorfik (`morph_id`, `morph_type`)|
|`nullableMorphs()`|8 + 16 Bytes|Sama dengan `morphs()`, tapi bisa null|
|**🔹 Specialty Types**|||
|`enum(['value1', 'value2'])`|1 – 2 Bytes|Nilai tetap dalam array|
|`set(['value1', 'value2'])`|1 – 8 Bytes|Kumpulan nilai yang bisa dipilih lebih dari satu|
|`macAddress()`|6 Bytes|Menyimpan alamat MAC (`00:1A:2B:3C:4D:5E`)|
|`ipAddress()`|4 atau 16 Bytes|Menyimpan IPv4 (4 Bytes) atau IPv6 (16 Bytes)|
|`rememberToken()`|100 Bytes|Token untuk autentikasi pengguna|
|`vector()`|Bervariasi|Menyimpan data vektor|

---

### **📌 Kesimpulan**
