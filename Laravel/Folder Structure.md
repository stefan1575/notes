# Laravel Folder Structure

Laravel's MVC or Model-View-Controller corresponds to the following files.

1. Model - `/app/Models/`
2. View `/resources/views/`
3. Controller - `/app/Http/Controllers`

## `/app/`

Each file in this folder is declared with a namespace representing its location.

- `Http/` -
  - `Controllers/` -
  - `Requests/` -
- `Models/` -
  - `Illuminate\Database\Eloquent\Model` - Each new that we create is extended by this class to gain access to other methods.
    - `all` - fetches all of the items in the database.
    - `find` - fetches a specific item in the database with an argument describing its identifier
  - `Illuminate\Database\Eloquent\Factories\HasFactory`
- `Policies/` -
- `Providers/` -

<!-- ## `/bootstrap/` -->

<!-- ## `/config/` -->

## `/database/`

- `factories/`
- `migrations/`
- `seeders/`

<!-- ## `/public/` -->
<!-- ## `/resources/` -->

## `/routes/`

- `console.php`
- `web.php`

<!-- ## `/storage/` -->

<!-- ## `/tests/` -->

<!-- ## `/vendor/` -->
