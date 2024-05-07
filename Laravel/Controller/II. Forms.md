# Forms

The `@csrf` directive creates a hidden input token. When creating a form in Laravel, it needs to be included to prevent CSRF.

Since browsers only natively support POST and GET requests, the `@method` directive has to be included in the form to hint the request type when using the `patch` and `delete` methods of the `Route` class.

A common pattern in the `delete` method is to create a hidden form where the delete button has a `form` attribute referencing the `id` of the hidden form.

## I. Validation

The `required` attribute in each form input is not enough because a user can submit a form in the command line.

### Extracting Submitted Input

The `request` method gets an instance of the current request.

If an argument is specified, gets an input item from the request represented by the given string.

### Validation Schema

The `validate` method accepts an associative array describing the schema of the form validation. Each field has a property containing an array of rules describing its constraints. Chained after the `request` method call.

- `required` - the field cannot be empty
- `min:` - a field must contain at least a specified amount of characters.
- `confirmed` - check if the field with the same name with `_confirmation` appended to it has the same value.
- `Password` - a class where we can chain password specific form validation.

### Displaying Errors

The `$error` variable allows us to display the errors in a view when an error occurs.

Blade has the `@error` directive which listens to a field when an error occurs. Inside the directive, `$message` variable allows us to display the error message.

To learn more: `https://laravel.com/docs/validation`

## II. `store`

When a form is submitted, the `store` method of a controller is called by convention.

### Methods

- `create` - a static method of a Model that accepts an associative array representing the table entry that we want to create.
  - Used inside the `store` method of a controller after authentication and form validation.
- `redirect` - redirects the user to the specified url. Returned after a POST request.

Since the `validate` method returns an associative array containing the fields and the submitted input _(attributes)_, it can be passed as an argument inside the `create` method to prevent duplication.

### Form Submission

When submitting forms, you can only mass assign attributes that are specified in the `$fillable` property of a Model.

To allow mass assignment of every attribute, set the `$guarded` property to an empty array.

## II. `Auth`

- `login` - Logs in the user determined by the passed Model instance as its argument.
  - Used inside the `store` method where returned user in the `create` method can be used as its argument to prevent duplication.
- `logout` - Logs out the user.

### Login Registered Users

- `attempt` - Attempts to log in the user. Accepts an array of attributes to check if the credentials match. Returns a boolean.

For the `attempt` method, a common pattern is to throw a `ValidationException` error and call the `withMessages` error to specify which field and message to display when the credentials do not match.

```php
if (!Auth::attempt($attributes)) {
    throw ValidationException::withMessages([
        'email' => 'Sorry those credentials to not match',
    ]);
}
```

If a `ValidationException` occurs, it automatically redirects to the same form with the messages.

### Displaying Failed Input

When the form validation fails, we want to display the old user submitted form input.

A common pattern is to set the `:value` attribute of the form input field to an `old` function with the field we want to display.

### Displaying Views

- `@guest` - Displays the content only to guests
- `@auth` - Displays the content to logged in user.
  - For the logout button, it is good practice to use a POST request when logging out a user.

## III. Session Hijacking

After authentication, a good practice is to regenerate the token when signing in the user to prevent session hijacking.

```php
request()->session()->regenerate();
```
