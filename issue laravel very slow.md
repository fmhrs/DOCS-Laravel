# Issue laravel very slow
## Background Issue
Setiap saya menggunakan laravel entah mau menggunakan library atau tidak loading page hingga request livewire sangat terasa lambat, sampai akhirnya saya testing menggunakn simple route sebagai berikut.
```php
Route::get("blank", function(){
    return response()->json(
        ['message' => 'Hello World', 'status' => 'success'],
        200,
        [], JSON_PRETTY_PRINT
    );}
);
```
disini saya membuat sebuah routing dengan method GET yang mengarah ke url "127.0.0.1/blank", 
konsepnya setiap kita mengarah ke url tersebut maka laravel akan mereturn atau mengirimkan respon data ['message' => 'Hello World', 'status' => 'success'] dengan kode status 200 / success dalam bentu JSON.
secara logika harusnya hanya memerlukan waktu yang sangat singkat untuk mendapatkan data tersebut dikarenakan tidak ada loading image, data-base, view, dan lain sebagainya.
akan tetapi setiap saya mengecek waktu yang dibutuhkan untuk mendapatkan response tersebut memerlukan waktu minimal sebanyak 400ms.  

disini saya menggunakan `npm run dev` dan `php artisan serve` untuk menjalankan servernya. saya memiliki kecurigaan jangan jangan yang menyebabkan lemot adalah koneksi database mysql saya yang menggunakan xampp sebagai perantara.
namun saat saya membuat project baru saya mendapati response yang sama, sampai akhirnya saya ingat bahwa php yang saya gunakan bukanlah php dari official website tapi php dari xampp karena sering terdapat issue bila kita menggunakan php diluar xampp untuk menjalankan apache xampp maka akan ada error yang tidak terduga.
dari sana saya mencari tahu dan menemukansaran untuk menyalakan `opcache` php.

### Solution : activate opcache on php.ini 
cari php.ini di xampp/php (jika menggunakan xampp). kemudian cari kata opcache seperti kode di bawah dan hapus tanda `;` di depan setiap text tersebut bila ada
contoh sebelum `;zend_extension=opcache` dan sesudah `zend_extension=opcache` atau kalian juga dapat langsung mengcopy code dibawah dan paste di atas baris pertama yang mengandung text `opcache` agar lebih mudah
karena dari php.ini saya tedapat banyak comment di antara setiap `opcache` jadi cukup merepotkan

```ini
zend_extension=opcache

opcache.enable=1
opcache.enable_cli=1
opcache.memory_consumption=128
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=10000
opcache.revalidate_freq=2
opcache.fast_shutdown=1
```

setelah dilakukan pengubahan, restat vscode atau terminal kalian. kemudian kalian dapat periksa opcache telah berjalan di terminal kalian dengan menjalankan
`php -i | findstr "opcache"` atau `php -i | grep -i opcache` jika berhasil di dinstall maka akan muncul settingan opcache yang sedang bekerja. jika tidak muncul apa apa maka terminal belum mendeteksi opcache kalian dengan benar.

setelah itu kalian dapat menjalankan magicword pada laravel kalian sebagai berikut, dan jangan lupa untuk menonaktifkan throtle pada inspect element kalian
```cmd
php artisan config:clear
php artisan cache:clear
php artisan route:clear
php artisan view:clear
composer dump-autoload

php artisan config:cache
php artisan route:cache
php artisan view:cache
```

setelah itu kalian dapat menjalankan `npm run dev` dan `php artisan serve`
## Result
Info pengujian sistem
- menggunakan ryzen 5 5600g tanpa vga external,  
- menjalankan project dengan koneksi mysql milik xampp.  
- serta menjalankan `npm run dev` serta `php artisan serve` secara bersamaan
```C
# dengan menggunakan localhost/blank
initial connection blank: 0.47 ms +
initial response blank: 30 - 60 ms
initial connection favicon: 315 ms +
initial response favicon: 0.85 ms
blank(60 ms) + favicon(300ms) [300-400 ms]
```
```C
# dengan menggunakan 127.0.0.1/blank
initial connection blank: 0.26 ms +
initial response blank: 10 ms
initial connection favicon: 0.43 ms +
initial response favicon: 0.38 ms
blank(10 ms) + favicon(5) [10 ms]
```
