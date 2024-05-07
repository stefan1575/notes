# View - `resources/views`

In Laravel, by default views that are defined by the `.blade.php` file extension.

The HTML tag of the blade template is prefixed by `x-` followed by the component name.

## I. `$slot`

The `$slot` variable is a placeholder which allows a view that consumes the component to inject its own information.

_Named slots_ are created by creating variables which are consumed as an HTML tag prefixed with `x-slot:` followed by the variable name.

```php
// resources/views/Component/heading.blade.php
<div>
    <header>{{ $heading }}</header>
    <main>{{ $slot }}</main>
</div>

// resources/views/home.blade.php
<x-layout>
    <x-slot:heading>Home</x-slot:heading>
    <h1>This is the Home Page.</h1>
</x-layout>
```

## II. `view`

The `view` function loads the corresponding view with the same file name located in `/resources/views/`.

It accepts a second argument that allows us to pass Model specific data to the corresponding view.

- `latest` - reverses the order of queries.

```php
Route::get('/jobs', function () {
    return view('jobs', ['jobs' => Job::with('employer')->latest()->paginate(10)]);
});

// /resources/views/jobs.blade.php
<x-layout>
    <x-slot:heading>Jobs</x-slot:heading>
    <ul>
        @foreach ($jobs as $job)
            <li>
                <a class="text-blue-500 hover:underline" href="jobs/{{ $job['id'] }}">
                    <strong>{{ $job['title'] }}</strong>: Pays {{ $job['salary'] }} per year.
                </a>
            </li>
        @endforeach
    </ul>
</x-layout>
```

The URI of a wildcard is represented by curly braces where its name is called as an argument of the `$callback` function.

```php
// wildcard
Route::get('/jobs/{id}', function ($id) {
    return view('job', ['job' => Job::find($id)]);
});

// /resources/views/job.blade.php
<x-layout>
    <x-slot:heading>Job</x-slot:heading>
    <h2 class="font-bold text-lg">{{ $job['title'] }}</h2>
    <p>This job pays {{ $job['salary'] }} per year.</p>
</x-layout>
```

To summarize, the second argument of the `view` method is used to pass in data that will be consumed by the `$slot` variable.
