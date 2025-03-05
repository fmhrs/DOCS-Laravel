# Tailwind CSS
Cara installasi
- Sumber : https://tailwindcss.com/docs/installation/framework-guides/laravel/vite
Steps
- run : `npm install tailwindcss @tailwindcss/vite`
- setup vite.config.ts
```ts
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import tailwindcss from '@tailwindcss/vite';  // <---
 
export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'], // <---
            refresh: true,
        }),
        tailwindcss(), // <---
    ],
});
```
- tambahkan `@import` dan `@source` pada `./resources/css/app.css`
```css
@import "tailwindcss";
@source "../views";
```
- lalu pada view pasang `@vite('resources/css/app.css')`
```php
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    @vite('resources/css/app.css') // <---
  </head>
  <body>
    <h1 class="text-3xl font-bold underline">
      Hello world!
    </h1>
  </body>
</html>
```
- untuk menjalankan tailwind harus di build setiap kali ada penggunaan nama class baru yang ada di halaman view dengan cara `npm run build` jika ingin tidak build secara berulang cukup jalankan `npm run dev` dan `php artisan serve` pada 2 terminal yang berbeda, lalu build project pada saat telah selesai
