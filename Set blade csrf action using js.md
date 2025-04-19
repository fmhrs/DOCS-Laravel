# Set blade csrf action using js

**The Problem**
if you using `Route::resource()` on web laravel, PUT method will ask you second parameter there are using for the id from the data will be updated.  

there will not be a problem if we have 1 page just for the update that data. if we make edit data on read page (pagination) that can update data from modal, that will be challanging.  

to fix that you can write that `route(firstParam, secondParam)` on JS then you can write the second param as `:id` so they wont trhow error for not putting some value on that parameter, but you must replace that `:id` to the id that you want update.  



```blade
<form id="user-form" method="POST">
    @csrf
    @method('PUT')

    <input type="number" name="user_id"/>
    <button type="submit"> submit </button>
</form>
```
```js
let routeUrl = "{{ route('admin.user.update', ['user' => ':id']) }}".replace(':id', id);
$('#user-form').attr('action', routeUrl);
```
