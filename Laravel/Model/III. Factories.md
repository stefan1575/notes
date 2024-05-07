# Factories - `database/factories`

Automatically generates database entries. They are used outside of database seeders for testing.

The `HasFactory` trait allows a model to automatically generate database entries if the model has the same shape as the factory.

- `factory(n)->create()` - creates a database row with the provided amount as its argument

## `state`

Each model class has a `state` method that accepts a callback function which accepts an array of attributes.

The callback function returns an array where we can manually assign database specific database values.

## Database Seeders

Database seeders are invoked in the `run` method of the `DatabaseSeeder` to trigger multiple factory calls.

- `db:seed` - executes the seeder to populate the database.
  - `--class=` - only executes the given seeder class to populate the database.
- `make:seeder` - creates a new database seeder class to populate a target table.
- `--seed` - can also be used alongside migrations.

### `call`

Inside the `run` method of the `DatabaseSeeder` class, the other created seeders can be executed using the static `call` method with the seeder class as its argument.
