# DOCS-Laravel
 Kumpulan code untuk melakukan pengembangan framework PHP bernama Laravel


## Templating
```md
# Folder Structure
├── resources/
│   └── views/
│       ├── layouts/
│       │   └── master.blade.php
│       │   ├── navbar.blade.php
│       │   ├── sidebar.blade.php
│       │   └── footer.blade.php
│       ├── components/
│       ├── dashboard/
│       │   ├── index.blade.php
│       ├── home/
│       │   ├── index.blade.php
│       └── welcome.blade.php
```

#### mini titor  
penggunaan `{{ asset('css/custom-style.css') }}` mengarah ke directory `project > public > css > custom-style.css`  
untuk master tidak harus seperti berikut, sesuaikan saja dengan template bootstrap / tailwind masing-masing  

dashboard memanggil template dengan `@extends('layouts.master')` pada baris ke 1 dan tanpa tag penutup  
master memanggil layout head, nav, side, footer,  menggunakan `@include('layout.nama')`  
master menerima code dari dashboard melalui `@yield('nama_yield')`, dashboard memberikan code dengan `@section('nama_yield')` ditutup dengan `@endsection('nama_yield')`  
```blade
# Master
html
- nav
-- @yiled('title')
-- include('layouts.navbar')
-body
-- @include ('navbar) 
-- @include ('sidebar)
-- @yield ('content)
-- include('layouts.script')
-- @yield ('content)

# dashboard
- @extends('layouts.master')
- @section('title', 'Dashboard')
- @section('head') <link/> @endsection
- @section('content') <section/> @endsection
- @section('script') <script/> @endsection
```
```blade
# master.blade.php
<!DOCTYPE html>
<html lang="en">

  <head>
    <title>@yield('title') - nama project</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    @yield('head')

  </head>

  <body>

    @include('layouts.navbar')
    @yield('content')
    @include('layouts.footer')
  
    @yield('script')
  </body>
</html>
```

```blade
# dashboard
@extends('layouts.master')

@section('title', 'Dashboard')

@section('head')
   <link rel="stylesheet" href="{{ asset('css/custom-style.css') }}">
@endsection

@section('content')
  <section>
    ...
  </section>
@endsection


@section('script')
  <script src="{{ asset('js/custom.js') }}"></script>
@endsection
```
