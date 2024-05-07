# Mailable

- File Location: `app/Mail/`
- Import Path: `Illuminate\Support\Facades\Mail`

## I. Mailable Class

A mailable is created by extending the `Mailable` class.

- `content` - a returned view that will be displayed as an email to the user.
- `envelope` - contains the email header information.
  - `from` and `replyTo` can be specified to override the defaults.
- `attachments` - files to be included in the email.
- `__construct` -

All `public` properties within a mailable class including the model instance provided in the `__construct` parameter are available inside the view specified in the `content` method.

If we only want to allow a subset of properties of the model instance, we should should specify them in the `with` property of the returned `content` instance.

### Sending Mail

One drawback of sending mail using the `send` method is that it blocks other code from being executed.

A good practice is to send emails asynchronously by using the `queue` method instead.

## II. Configuration

The mail settings can be configured in either the `config/mail.php` or `.env` file.

By default, mail is sent to the `storage/logs/laravel.log` and they are sent synchronously.

## III. Queue

The `dispatch` function accepts a callback function that will be queued. The execution of the dispatch function can be delayed by chaining it with the `delay` method.

### Workers

Queues are executed by processes called _workers_ that execute tasks once they are encountered. It can be created manually using the `queue:work` command.

The worker should be restarted when making changes to the codebase since it loads everything into memory.

## IV. Jobs

- File Location: `app/Jobs`

The `handle` function is executed when the static `dispatch` method of the created Job class is ran.

The static `dispatch` method accepts an argument that has the same shape as the instance defined in the `__constructor` argument. It can be read inside the handle method.
