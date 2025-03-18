# Livewire
docs url : https://livewire.laravel.com/docs/quickstart
Install using composer: `composer require livewire/livewire`

## Creating Simple Livewire component
Artisan : `php artisan make:livewire NamaComponent`
kode tersebut akan membuat 2 buah file:
- `app/Livewire/NamaComponent.php`
- `resources/views/livewire/NamaComponent.blade.php`

**Controller** : app/Livewire/NamaComponent.php
```php
class NamaComponent extends Component
{
    public $count = 1;
 
    public function increment()
    {
        $this->count++;
    }
 
    public function decrement()
    {
        $this->count--;
    }
 
    public function render()
    {
        return view('livewire.counter');
    }
}
```

**View Live** : resources/views/livewire/NamaComponent.blade.php
```blade
<div>
  <h1>testing product table using increment function</h1>

  <div class="join w-full">
    <button class="join-item btn" wire:click="decrement">decrement</button>
    <button class="join-item btn text-primary grow">{{ $count }}</button>
    <button class="join-item btn" wire:click="increment">increment</button>
  </div>
</div>
```

**Calling Livewire Component from blade**
 `<livewire:NamaComponent :count="1" />`

**Loading state saat fetching data** : `wire:loading`
```blade
<div class="join w-full ">
    <button class="join-item btn" wire:click="decrement" wire:loading.attr="disabled">decrement</button>
    <button class="join-item btn text-primary grow">{{ $count }}</button>
    <button class="join-item btn" wire:click="increment" wire:loading.attr="disabled">increment</button>
  </div>
```
