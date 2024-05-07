# Controller - `app/Http/Models`

## I. Router

Responsible for listening to requests and loading the corresponding controller.

- File Location: `/routes/web.php`
- Import Path: `Illuminate\Support\Facades\Route`

The `Route` class allows us to register routes that corresponding HTTP verb. Each method takes a `$uri` and `$callback` as its argument.

- `$uri` - The `uri` to listen for.
- `$callback` - A callback function that runs when the `$uri` and the corresponding request matches

### Route Model Binding

When the string is enclosed in curly braces, Laravel will automatically detect that it is a wildcard.

When the wildcard name has the same name as the variable with the model type in the callback function, it will return an instance of the specified model.

By default the wildcard uses the `id` of the instance in the URL.

```php
Route::get('/jobs/{job}', function (Job $job) {
    return view('jobs.show', ['job' => $job]);
});
```

To use another table column, append a colon to the wildcard name followed by the table column name.

## II. Controller Classes

### A. `Route::resource`

The `resource` method automatically defines the request type, path, and the respective static method using the following convention.

- `uri` - the index URI
- `controller` - the controller of the index URI
- `options` - an array containing the following properties.
  - `only` - an array that specifies which actions to include
  - `except` - an array that specify which actions to exclude

| Verb   | Path              | Action/Method |
| ------ | ----------------- | ------------- |
| GET    | `/uri`            | `index`       |
| GET    | `/uri/create`     | `create`      |
| GET    | `/uri/{job}`      | `show`        |
| POST   | `/uri`            | `store`       |
| GET    | `/uri/{job}/edit` | `edit`        |
| PATCH  | `/uri/{job}`      | `update`      |
| DELETE | `/uri/{job}`      | `destroy`     |

### B. `Route::controller`

The `controller` static method defines a common controller for all the routes. This is used if the resource doesn't use Laravel's RESTful conventions.

- `group` - a callback function where the routes are defined. It is chained after the `controller` method.

```php
Route::controller(JobController::class)->group(function () {
    Route::get('/jobs', 'index');
    Route::get('/jobs/create', 'create');
    Route::get('/jobs/{job}', 'show');
    Route::post('/jobs', 'store');
    Route::get('/jobs/{job}/edit', 'edit');
    Route::patch('/jobs/{job}', 'update');
    Route::delete('/jobs/{job}', 'destroy');
});
```

For resources that only perform a get request, the `Route::view` method listens for a get request to the specified URI and loads a view.

### Listing routes

- `route:list` - lists all the routes specified in `routes/web.php`
  - `--except-vendor` - excludes the files in the `vendor` folder.
