# Configuration Files

- `composer.json`
  - `autoload` - Allows us to import and export files with the use of `namespace` and the `use` keyword with the appended path of the file that we want to use.
    - `psr-4`
      - convention for autoloading files.
      - an object that contains properties as the namespace and values as the corresponding path.
- `.env`
  - `DB_CONNECTION` - describes which database to use.
  - `APP_DEBUG` - a boolean indicating whether it is used in development or in production
