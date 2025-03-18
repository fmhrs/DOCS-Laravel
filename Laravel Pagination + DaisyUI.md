# Livewire Pagination

docs url : https://livewire.laravel.com/docs/pagination

**Penting**: pasangkan `use Livewire\WithPagination;` dan `use WithPagination;` pada controller live wire, serta gunakan artian `php artisan livewire:publish --pagination` untuk styling khusus livewire

**Instalasi**: contoh menggunakan nama AdminProductTable
artisan: `php artisan make:livewire AdminProductTable`

Controller: `App/Http/Controllers/Livewire/AdminProductTable`
```php
use App\Models\Product; // <-- penting

class AdminProductTable extends Component
{
    use WithPagination; // <-- penting
    
    public $search = '';
    public $category = ''; 

    public function filterTable()
    {
        $this->resetPage(); // Reset ke halaman pertama saat search berubah
    }
    public function render()
    {
        $queryProduct = Product::with('product_category');
        if($this->search){
            $queryProduct->whereAny(['name', 'price', 'category'], 'like', "%{$this->search}%");
        }

        if ($this->category) {
            $queryProduct->where('product_category_id', $this->category);
        }

        $data = [
            'product_categories' => ProductCategory::all(),
            'products' => $queryProduct->paginate(5)
        ];

        return view('livewire.admin-produk-table', $data);
    }
}
```

View: `resource/views/livewire/admin-produk-table`
```blade
<div>
  {{-- FILTER TABLE --}}
  <form wire:submit="filterTable" class="ms-auto mb-4 flex justify-end gap-2">
    <label class="input input-sm flex-2 sm:flex-none">
      <svg class="h-[1em] opacity-50" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
        <g stroke-linejoin="round" stroke-linecap="round" stroke-width="2.5" fill="none" stroke="currentColor">
          <circle cx="11" cy="11" r="8"></circle>
          <path d="m21 21-4.3-4.3"></path>
        </g>
      </svg>
      <input type="search" class="grow" name="search" placeholder="Search items"
        value="{{ request()->query('search') }}" wire:model="search" />
    </label>

    <select name="product_category" class="select select-sm flex-1 sm:flex-none sm:max-w-36" wire:model="category">
      <option value="">All Categories</option>
      @foreach ($product_categories as $cat)
        <option value="{{ $cat->id }}" {{ request('product_category') == $cat->id ? 'selected' : '' }}>
          {{ $cat->name }}
        </option>
      @endforeach
    </select>

    <button class="btn btn-sm" type="submit">submit</button>
  </form>

  {{-- TABLE PRODUK --}}
  <div class="overflow-x-auto rounded-box border border-base-content/5 bg-base-100 mb-4">
    <table class="table table-sm table-pin-rows table-pin-cols min-w-lg">
      <thead>
        <tr>
          <th>Category</th>
          <th>Name</th>
          <th>Price</th>
          <th class="w-28 -z-10">Action</th>
        </tr>
      </thead>
      <tbody>
        @foreach ($products as $row => $product)
          <tr>
            <td>{{ $product->product_category->name }}</td>
            <td>{{ $product->name }}</td>
            <td>Rp.{{ $product->price }}</td>
            <td>
              <button class="btn btn-xs"><x-svg.settings class="w-3 me-2" />Edit</button>
            </td>
          </tr>
        @endforeach
      </tbody>
    </table>
  </div>
  {{ $products->onEachSide(2)->links() }}
</div>
```

Custom Style Livewire pagination using DaisyUI 
**artisan**: `php artisan livewire:publish --pagination`
nanti akan terbentuk banyak file di `resource/views/vendor/livewire`, edit tailwind file menjadi
```blade
@php
if (! isset($scrollTo)) {
    $scrollTo = 'body';
}

$scrollIntoViewJsSnippet = ($scrollTo !== false)
    ? <<<JS
       (\$el.closest('{$scrollTo}') || document.querySelector('{$scrollTo}')).scrollIntoView()
    JS
    : '';
@endphp

<div>
    @if ($paginator->hasPages())
        <nav role="navigation" aria-label="Pagination Navigation" class="flex items-center justify-between">
            <div class="flex justify-between flex-1 sm:hidden">
                <span>
                    @if ($paginator->onFirstPage())
                        <span class="relative inline-flex items-center btn btn-disabled">
                            {!! __('pagination.previous') !!}
                        </span>
                    @else
                        <button type="button" wire:click="previousPage('{{ $paginator->getPageName() }}')" x-on:click="{{ $scrollIntoViewJsSnippet }}" wire:loading.attr="disabled" dusk="previousPage{{ $paginator->getPageName() == 'page' ? '' : '.' . $paginator->getPageName() }}.before" class="relative inline-flex items-center btn">
                            {!! __('pagination.previous') !!}
                        </button>
                    @endif
                </span>

                <span>
                    @if ($paginator->hasMorePages())
                        <button type="button" wire:click="nextPage('{{ $paginator->getPageName() }}')" x-on:click="{{ $scrollIntoViewJsSnippet }}" wire:loading.attr="disabled" dusk="nextPage{{ $paginator->getPageName() == 'page' ? '' : '.' . $paginator->getPageName() }}.before" class="relative inline-flex items-center btn">
                            {!! __('pagination.next') !!}
                        </button>
                    @else
                        <span class="relative inline-flex items-center btn btn-disabled">
                            {!! __('pagination.next') !!}
                        </span>
                    @endif
                </span>
            </div>

            <div class="hidden sm:flex-1 sm:flex sm:items-center sm:justify-between flex-wrap gap-2">
                <div>
                    <p class="text-sm leading-5">
                        <span>{!! __('Showing') !!}</span>
                        <span class="font-medium">{{ $paginator->firstItem() }}</span>
                        <span>{!! __('to') !!}</span>
                        <span class="font-medium">{{ $paginator->lastItem() }}</span>
                        <span>{!! __('of') !!}</span>
                        <span class="font-medium">{{ $paginator->total() }}</span>
                        <span>{!! __('results') !!}</span>
                    </p>
                </div>

                <div class="ms-auto">
                    <span class="relative z-0 inline-flex rtl:flex-row-reverse join">
                        <span>
                            {{-- Previous Page Link --}}
                            @if ($paginator->onFirstPage())
                                <span aria-disabled="true" aria-label="{{ __('pagination.previous') }}">
                                    <span class="relative inline-flex items-center px-2 py-2 join-item btn" aria-hidden="true">
                                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                                            <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
                                        </svg>
                                    </span>
                                </span>
                            @else
                                <button type="button" wire:click="previousPage('{{ $paginator->getPageName() }}')" x-on:click="{{ $scrollIntoViewJsSnippet }}" dusk="previousPage{{ $paginator->getPageName() == 'page' ? '' : '.' . $paginator->getPageName() }}.after" class="relative inline-flex join-item btn" aria-label="{{ __('pagination.previous') }}">
                                    <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                                        <path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />
                                    </svg>
                                </button>
                            @endif
                        </span>

                        {{-- Pagination Elements --}}
                        @foreach ($elements as $element)
                            {{-- "Three Dots" Separator --}}
                            @if (is_string($element))
                                <span aria-disabled="true">
                                    <span class="relative inline-flex join-item btn">{{ $element }}</span>
                                </span>
                            @endif

                            {{-- Array Of Links --}}
                            @if (is_array($element))
                                @foreach ($element as $page => $url)
                                    <span wire:key="paginator-{{ $paginator->getPageName() }}-page{{ $page }}">
                                        @if ($page == $paginator->currentPage())
                                            <span aria-current="page">
                                                <span class="relative inline-flex join-item btn text-primary">{{ $page }}</span>
                                            </span>
                                        @else
                                            <button type="button" wire:click="gotoPage({{ $page }}, '{{ $paginator->getPageName() }}')" x-on:click="{{ $scrollIntoViewJsSnippet }}" class="relative inline-flex join-item btn " aria-label="{{ __('Go to page :page', ['page' => $page]) }}">
                                                {{ $page }}
                                            </button>
                                        @endif
                                    </span>
                                @endforeach
                            @endif
                        @endforeach

                        <span>
                            {{-- Next Page Link --}}
                            @if ($paginator->hasMorePages())
                                <button type="button" wire:click="nextPage('{{ $paginator->getPageName() }}')" x-on:click="{{ $scrollIntoViewJsSnippet }}" dusk="nextPage{{ $paginator->getPageName() == 'page' ? '' : '.' . $paginator->getPageName() }}.after" class="relative inline-flex join-item btn " aria-label="{{ __('pagination.next') }}">
                                    <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                                        <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
                                    </svg>
                                </button>
                            @else
                                <span aria-disabled="true" aria-label="{{ __('pagination.next') }}">
                                    <span class="relative inline-flex join-item btn" aria-hidden="true">
                                        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
                                            <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
                                        </svg>
                                    </span>
                                </span>
                            @endif
                        </span>
                    </span>
                </div>
            </div>
        </nav>
    @endif
</div>

```
