# Routing 

Semua url dan method pada laravel di atur pada halaman `route/web.php` terdapat 6 metode yang dapat digunakan pada laravel

```blade
<?php
use Illuminate\Support\Facades\Route;

Route::get($uri, $callback); // Digunakan untuk mengambil atau menampilkan data
Route::post($uri, $callback); // Digunakan untuk mengirim atau menyimpan data baru
Route::put($uri, $callback); // Digunakan untuk memperbarui data secara keseluruhan
Route::patch($uri, $callback); // Digunakan untuk memperbarui data secara parsial
Route::delete($uri, $callback); // Digunakan untuk menghapus data
Route::options($uri, $callback); // Digunakan untuk mendapatkan opsi HTTP yang tersedia untuk sebuah endpoint
```

**contoh penggunaan**
```blade
<?php
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\DashboardController;

Route::get("/dashboard", function(){ return view ('dashboard')} );

// atau menggunakan controller
Route::get("/dashboard", [DashboardController::class, 'index']);
```

**fitur name pada routing**
```
<?php
use Illuminate\Support\Facades\Route;

Route::get("/dashboard", function(){ return view ('dashboard')})->name('dashboard') ;
Route::get("/admin/dashboard", function(){ return view ('admin.dashboard')})->name('admin.dashboard') ;
```

```blade
<a href="{{ Route('dashboard')}}"> dashboard </a>
<a href="{{ Route('admin.dashboard')}}"> admin dashboard </a>
```

**middleware**
membuat halaman dapat diakses hanya oleh orang yang telah login, jika belum login maka diarahkan ke halaman login
NB: redirect ke login hanya berhasil apabila telah membuat `->name('login')` pada salah satu route.
```
<?php
use Illuminate\Support\Facades\Route;

Route::middleware(['auth'])->get("/admin/dashboard", function(){ return view ('admin.dashboard')})->name('admin.dashboard');

Route::get('/login', function() {return view('login')})->name('login');
```
untuk cara login logout dapat lihat di dokumentasi [Manually Authenticating Users](https://laravel.com/docs/12.x/authentication#authenticating-users)

**prefix**
```blade

Route::middleware(['auth'])->prefix('admin/')->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index'])->name('admin.dashboard');
    Route::get('/user')->name('admin.user');
    Route::get('/product')->name('admin.product');
    Route::get('/finance')->name('admin.finance');
    Route::get('/profiile')->name('admin.profile');
});
```

**prefix using name**
on progress

**resource** on progress
```
Route::resources([
    'photos' => PhotoController::class,
    'posts' => PostController::class,
]);
```
#### [Actions Handled by Resource Controllers](https://laravel.com/docs/12.x/controllers#actions-handled-by-resource-controllers)

|Verb|URI|Action|Route Name|
|---|---|---|---|
|GET|`/photos`|index|photos.index|
|GET|`/photos/create`|create|photos.create|
|POST|`/photos`|store|photos.store|
|GET|`/photos/{photo}`|show|photos.show|
|GET|`/photos/{photo}/edit`|edit|photos.edit|
|PUT/PATCH|`/photos/{photo}`|update|photos.update|
|DELETE|`/photos/{photo}`|destroy|photos.destroy|
