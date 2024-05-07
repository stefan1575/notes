# Authentication

When running `laravel new <app-name>` and select Breeze, it automatically creates a project that is scaffolded with user authentication.

## I. Manual Authentication

When allowing CRUD operations to only a subset of users (i.e. displaying view returned by the `edit` method), we have to guard against guests and other registered users.

When the user is a guest, redirect them to the login page.

### `is`/`isNot`

A relationship has to be created to determine if the credentials of the user match with the table entry that we want to manipulate.

This can be done by creating a relationship method with a `belongsTo` method call with the model describing the related table entry.

The `$model->is` method determines if two models have the same ID and belong to the same table.

It can be also chained with the `belongsTo` relationship method.

```php
// Job belongsTo Employer
// Employer belongsTo User
public function edit(Job $job)
{
    if (Auth::guest()) {
        return redirect('/login');
    }

    if ($job->employer->user->isNot(Auth::user())) {
        abort(403);
    }

    return view('jobs.edit', ['job' => $job]);
}
```

## II. Gates

Import Path: `Illuminate\Support\Facades\Gate`

- `define` - defines a Gate and callback function that returns a boolean which compares the currently authenticated user from another model ID.
  - The callback function accepts an automatically injected `Auth::user()` and another model.
  - If we want the check to be performed if the user is not signed in, we should either set the `$user` variable to null or make the type optional.
- `authorize` - runs the callback function of the specified Gate
  - redirects to the same resource if `Auth::guest()` is true.
  - returns `abort(403)` if the callback returns false.
- `allows`/`denies` - runs the callback function and returns a boolean.
  - Used as a conditional where we run different behavior from the `authorize` method.

A common convention is to define Gates in the `AppServiceProvider` to allow access throughout the application.

### `can`/`cannot`

The `$model->can` method runs the callback function of the specified Gate or Policy similar to `allows`/`denies`.

The corresponding `@can` blade directive accepts a Gate and model as an argument. It is used to conditionally display content to the user depending if the callback passes or fails.

## III. Route-Level Authorization

One drawback of using `Route::resource` is applying middleware since as it will get applied to every single route.

When using the `auth` middleware, it looks for a named route called `login` which can be done using the `name` method.

Routes also have a `can` method that accepts a string representing the Gate name and a model

## IV. Policies

File Location: `app/Policies/`

- `make:policy` - creates a policy for the specified Model.

Policies are classes where authorization logic of a particular model or route/resource are organized.

```php
class JobPolicy
{
    public function edit(User $user, Job $job): bool
    {
        return $job->employer->user->is($user);
    }
}
```

A convention is default Gates and use Policies for non-trivial applications.
