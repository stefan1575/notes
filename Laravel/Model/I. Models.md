# Model - `app/Models`

Where application specific logic is encapsulated such as CRUD and database operations.

## I. Model Creation

Eloquent is an ORM that allows us to map a database table to a class in your PHP code by extending the `Model` class.

There are two ways to map a class to and a database table:

1. A class name that describes the singular form of the table name.
2. Add a protected property `$table` and assign it the table name.

Import path: `Illuminate\Database\Eloquent\Model`

### Creation

- `make:model` - creates a new Model with an identifier as its class name.
  - `-m` - creates a corresponding migration
  - `-f` - creates a corresponding factory

## II. Database Operations

Each model has the following static methods:

- `all` - fetches all of the items in the database.
- `find` - fetches a specific item in the database with an argument describing its identifier.
  - `delete` - deletes the selected database row.
- `create` - create a new database row with an associative array describing the column name and its value.
  - By default, its only executed succesfully when the property that you are trying to create specified in the `$fillable` array.
- `save` - updates the changed values of the selected database row.

## III. Establishing relationships

### A. One-to-many

- `hasMany` - a model is the parent to multiple child models
- `belongsTo` - defines the inverse of the `hasMany` relationship

```php
class Job extends Model {
    public function employer()
    {
        return $this->belongsTo(Employer::class);
    }

}

class Employer extends Model {
    public function jobs()
    {
        return $this->hasMany(Job::class);
    }
}
```

Each database row has a reference to their relationships which is accessed invoking the relationship method as a _property_.

```php
$job = App\Models\Employer::first();
$job->employer->name; // returns the database row

$employer = App\Models\Job::first();
$employer->jobs; // returns a collection
```

### B. Many-to-many

- `belongsToMany` - represents a relationship between two models where each model can be associated with multiple instances of another model.

#### Pivot Tables

Pivot tables are schemas that consists foreign keys that describe many-to-many relationships.

It is used when a relationship cannot simply be defined by just one table.

They are intermediary tables that hold the foreign keys of the two related tables.

```php
// To enforce the constraints in SQLite, run the query "PRAGMA foreign_keys=on"
Schema::create('job_tag', function (Blueprint $table) {
    $table->id();
    $table->foreignIdFor(Job::class, 'job_listing_id')->constrained()->cascadeOnDelete();
    $table->foreignIdFor(Tag::class)->constrained()->cascadeOnDelete();
    $table->timestamps();
});
```

Note that the resulting naming convention comes from the singular form of the connecting tables and sort them in alphabetical order separated by an underscore.

### C. Non-conventional ID names

The `foreignIdFor` creates a foreign key id that is associated given model.

#### One-to-many

If the model doesn't have a conventional id name (i.e. specified using the `$table` property)., it has to be specified as the second argument.

This is specified in the migration schema to create relationships between other tables.

```php
$table->foreignIdFor(Job::class, 'job_listing_id');
```

#### Many-to-many

In many-to-many relationships, the id name is specified by either the `relatedPivotKey` or `foreignPivotKey`

- `relatedPivotKey` - the associated model owns the key itself.
- `foreignPivotKey` - does not belong to the model but is only associated with it.

```php
class Tag extends Model
{
    public function jobs()
    {
        return $this->belongsToMany(Job::class, relatedPivotKey: 'job_listing_id');
    }
}

class Job extends Model
{
    public function tags()
    {
        return $this->belongsToMany(Tag::class, foreignPivotKey: "job_listing_id");
    }
}
```

## IV. Manipulating pivot tables

The relationship method is used as a method when chaining it with database operations in the pivot table

- `attach` - creates a new entry in the pivot table with fetched database item and its argument. It accepts the either an id or another database entry.

```php
use App\Models\Tag;

$tag = Tag::find(1);
$tag->jobs()->attach(7); // $tag->jobs()->attach(Tag::find(7));
```

- `get` - executes the given query. It can also be used to re-run the query to get the latest table.
- `pluck` - returns a collection containing all entries with specified column name of the fetched database collection.

## V. Lazy/Eager loading

By default, when a relationship is referenced, a new SQL query will be performed.

This causes performance issues in the context of a loop since a separate query is ran for each iteration.

### Disabling lazy loading

Lazy loading is enabled by invoking `Model::preventLazyLoading()` at the `boot` method of the `AppServiceProvider`.

When lazy loading occurs while this setting is enabled, it will result in an exception error.

## VI. Pagination

When you use the `get` method to execute queries causes memory issues as the table entries increases.

- `paginate` - executes the entire query where each entry is loaded by the given amount per page.
- `links` - displays the links for the paginated queries.
- `simplePaginate` - displays only the previous and next button for the paginated queries.
- `cursorPaginate` - the same as `simplePaginate` but the page number cannot be accessed in the URL, used for performance.

### Changing the default `links` view

The `vendor:publish` command allows us to manipulate the appearance of the `links` view.

The setting is enabled by invoking certain methods at the `Paginator` class in the `boot` method of the `AppServiceProvider`.
