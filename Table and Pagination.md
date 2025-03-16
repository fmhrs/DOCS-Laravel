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

## Pagination With Livewire
...
