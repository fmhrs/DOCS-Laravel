Lucide Icon url : https://lucide.dev/icons

Proses pemasangan icon lucide ke framework laravel menggunakan metode svg dan laravel components

**Steps**
1. pilih icon sebagai contoh "https://lucide.dev/icons/chevron-right"
2. buat folder svg pada `/views/components` lalu pada dir svg tersebut buat blade dengan nama icon  
   `ex: /views/components/svg/chevron-right.blade.php`
3. lalu copy lucide icon svg dan paste kedalam file svg tersebut  
```php title:chevron-right.blade.php
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor"
  stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-chevron-right">
  <path d="m9 18 6-6-6-6" />
</svg>
```
4. copy classname svg lalu hapus atribute dan tambahkan serta masukkan ke kode berikut `{{ $attributes->merge(['class' => 'class_icon_lucide']) }}`. kode ini berguna agar dapat dapat menambahkan class saat components di gunakan pada view.
```php title:chevron-right.blade.php
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor"
  stroke-width="2" stroke-linecap="round" stroke-linejoin="round" {{ $attributes->merge(['class' => 'lucide lucide-chevron-right']) }}>
  <path d="m9 18 6-6-6-6" />
</svg>
```
5. cara menggunakan pada view tinggal tuliskan tag dengan awalan "x-" lalu diikuti nama file component.   
   sebagai contoh `<x-svg.chevron-right class="me-2"/>`
