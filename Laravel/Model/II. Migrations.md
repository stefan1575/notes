# Migrations - `database/migrations`

Migrations represent the schema for the corresponding database table.

- `migrate`- used when making changes to the schema or fetching latest updates
  - `migrate:fresh` - drop all tables and re-run migrations, used for testing.
- `make:migration` - creates a new migration where the user will create its schema and and populate its database data

## `up`

The `up` method is invoked when the migration is ran. It can contain multiple `Schema` creation calls.

The `Schema` class accepts a string representing the table name and a callback function representing the shape of the database table.

A table field is created by chaining the `$table` variable with the type of the field we want to create.

- `id`
- `foreignIdFor`
- `string`
- `timestamps`

## `down`

The `down` method is invoked when the migration is dropped using one of the `migrate` commands is executed.

If there are multiple `Schema` creation calls, the tables have to be specified as well.
