# Pagination

Laravel secara default menyediakan fitur pagination, cara setup sederhana adalah sebagai berikut

**controller**
```php
public function index(Request $request){
  $data = [
    'products' => Product::simplePaginate(5) // hanya ada tombol next and previous
    // or
    'products' => Product::Paginate(5) // menyediakan data page number of table
  ];

  return view('admin.product', $data);
}
```

**view**
```blade
<div class="overflow-x-auto rounded-box border border-base-content/5 bg-base-100 mb-4">
  <table class="table table-pin-rows table-pin-cols min-w-lg">
  <thead>
    <tr>
    <th>Category</th>
    ...
    <th class="w-28 -z-10">Action</th>
    </tr>
  </thead>
  <tbody>
    @foreach ($products as $row => $product)
    <tr>
      <td>{{ $product->product_category->name }}</td>
      ...
      <td>
      <button class="btn btn-md"><x-svg.settings class="w-3 me-2" /> edit</button>
      </td>
    </tr>
    @endforeach
  </tbody>
  </table>
</div>
{{ $products->links() }} // this is the simpple way for paginating
```

## Pagination With Filter
**controller**
```php
public function index(Request $request){
  $search = $request->input('search');
  $category  = $request->input('product_category');
  
  // filtering by search
  $queryProduct = Product::with('product_category');
  if($search){
    $queryProduct->where('name', 'like', "%{$search}%");
  }
  
  // filtering by id category
  if ($category) {
    $queryProduct->where('product_category_id', $category);
  }
  
  $data = [
    'products' => $queryProduct->paginate(2)
  ];
  
  return view('admin.product', $data);
}
```

**view**
```blade
<form method="GET" action="{{ route('admin.product.index') }}">
  <input type="text" name="search" value="{{ request('search') }}" placeholder="Cari Produk">
  
  <select name="product_category">
    <option value="">-- Semua Kategori --</option>
    @foreach ($product_categories as $cat)
    <option value="{{ $cat->id }}" {{ request('product_category') == $cat->id ? 'selected' : '' }}>
      {{ $cat->name }}
    </option>
    @endforeach
  </select>
  
  <button type="submit">Filter</button>
</form>

<div class="overflow-x-auto rounded-box border border-base-content/5 bg-base-100 mb-4">
    ... <table> ...
</div>

{{ $products->appends(request()->query())->links() }}
```

## Limitting number 
```blade
{{ $products->onEachSide(2)->links() }}
```
![Pasted image 20250317232949](https://github.com/user-attachments/assets/e059fc0a-003b-40d5-abdc-3e539f20ca44)

## Customize Data Table
link: [Customizing the Pagination View](https://laravel.com/docs/12.x/pagination#customizing-the-pagination-view)

**costumize default** 
```php
php artisan vendor:publish --tag=laravel-pagination
```
nanti akan muncul beberapa file di `resource/views/vendor/pagination`  
![Pasted image 20250317230929](https://github.com/user-attachments/assets/86e44e16-353c-4752-96b1-023bd514311d)  

jika menggunakan tailwind edit bagian file yang memiliki nama tailwind. ini adalah settingan saya `tailwind.blade.php` menggunakan daisyui. 
```blade
@if ($paginator->hasPages())
  <nav role="navigation" aria-label="{{ __('Pagination Navigation') }}" class="flex items-center justify-between">
    <div class="flex justify-between flex-1 sm:hidden">
      @if ($paginator->onFirstPage())
        <span
          class="relative inline-flex items-center px-4 py-2 text-sm font-medium text-gray-500 bg-white border border-gray-300 cursor-default leading-5 rounded-md dark:text-gray-600 dark:bg-gray-800 dark:border-gray-600">
          {!! __('pagination.previous') !!}
        </span>
      @else
        <a href="{{ $paginator->previousPageUrl() }}"
          class="relative inline-flex items-center px-4 py-2 text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 rounded-md hover:text-gray-500 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 dark:bg-gray-800 dark:border-gray-600 dark:text-gray-300 dark:focus:border-blue-700 dark:active:bg-gray-700 dark:active:text-gray-300">
          {!! __('pagination.previous') !!}
        </a>
      @endif

      @if ($paginator->hasMorePages())
        <a href="{{ $paginator->nextPageUrl() }}"
          class="relative inline-flex items-center px-4 py-2 ml-3 text-sm font-medium text-gray-700 bg-white border border-gray-300 leading-5 rounded-md hover:text-gray-500 focus:outline-none focus:ring ring-gray-300 focus:border-blue-300 active:bg-gray-100 active:text-gray-700 transition ease-in-out duration-150 dark:bg-gray-800 dark:border-gray-600 dark:text-gray-300 dark:focus:border-blue-700 dark:active:bg-gray-700 dark:active:text-gray-300">
          {!! __('pagination.next') !!}
        </a>
      @else
        <span
          class="relative inline-flex items-center px-4 py-2 ml-3 text-sm font-medium text-gray-500 bg-white border border-gray-300 cursor-default leading-5 rounded-md dark:text-gray-600 dark:bg-gray-800 dark:border-gray-600">
          {!! __('pagination.next') !!}
        </span>
      @endif
    </div>

    <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between flex-wrap gap-2">
      <div>
        <p class="text-sm text-gray-700 leading-5 dark:text-gray-400">
          {!! __('Showing') !!}
          @if ($paginator->firstItem())
            <span class="font-medium">{{ $paginator->firstItem() }}</span>
            {!! __('to') !!}
            <span class="font-medium">{{ $paginator->lastItem() }}</span>
          @else
            {{ $paginator->count() }}
          @endif
          {!! __('of') !!}
          <span class="font-medium">{{ $paginator->total() }}</span>
          {!! __('results') !!}
        </p>
      </div>

      <div class="ms-auto">
        <span class="join relative z-0 inline-flex rtl:flex-row-reverse">
          {{-- Previous Page Link --}}
          @if ($paginator->onFirstPage())
            <span aria-disabled="true" aria-label="{{ __('pagination.previous') }}">
              <button class="join-item btn relative inline-flex cursor-default" aria-hidden="true">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd"
                    d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                    clip-rule="evenodd" />
                </svg>
              </button>
            </span>
          @else
            <a href="{{ $paginator->previousPageUrl() }}" rel="prev" class="join-item btn relative inline-flex"
              aria-label="{{ __('pagination.previous') }}">
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd"
                  d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                  clip-rule="evenodd" />
              </svg>
            </a>
          @endif

          {{-- Pagination Elements --}}
          @foreach ($elements as $element)
            {{-- "Three Dots" Separator --}}
            @if (is_string($element))
              <span aria-disabled="true">
                <span class="join-item btn relative inline-flex">{{ $element }} </span>
              </span>
            @endif


            {{-- Array Of Links --}}
            @if (is_array($element))
              @foreach ($element as $page => $url)
                @if ($page == $paginator->currentPage())
                  <span aria-current="page">
                    <span class="join-item btn text-primary relative inline-flex">{{ $page }}</span>
                  </span>
                @else
                  <a href="{{ $url }}" class="join-item btn relative inline-flex"
                    aria-label="{{ __('Go to page :page', ['page' => $page]) }}">
                    {{ $page }}
                  </a>
                @endif
              @endforeach
            @endif
          @endforeach

          {{-- Next Page Link --}}
          @if ($paginator->hasMorePages())
            <a href="{{ $paginator->nextPageUrl() }}" rel="next" class="join-item btn relative inline-flex"
              aria-label="{{ __('pagination.next') }}">
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd"
                  d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                  clip-rule="evenodd" />
              </svg>
            </a>
          @else
            <span aria-disabled="true" aria-label="{{ __('pagination.next') }}">
              <span class="join-item btn relative inline-flex " aria-hidden="true">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                  <path fill-rule="evenodd"
                    d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                    clip-rule="evenodd" />
                </svg>
              </span>
            </span>
          @endif
        </span>
      </div>
    </div>
  </nav>
@endif
```
## Pagination With Livewire
...
