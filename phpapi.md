# REST APIS with PHP

We will need to use composer (PHP package manager).

## Creating a composer.json

```php
composer init
```

## Installing JWT & Autoload

```php
composer require firebase/php-jwt
```

You can also find the package on [packagist.org](https://packagist.org/packages/firebase/php-jwt)

Now we want to automatically find classes and their corresponding files without having to require all of them manually.
To do this we need to use php's Autoload

```php
require __DIR__ . '/vendor/autoload.php';
```

## File Directory

```bash

```

## index.php

Coming from Node.js, this is typically our server.

```php
<?php>
require __DIR__ . '/vendor/autoload.php'; //for autoloading
require 'routes';
```

## Endpoints

Create an 'endpoints' directory.

In here, are the controllers. We will create a UserController

```php
<?php
 class User {
    // this is the controller
    public function __construct() {

    }

    // this is the create method
    public function create() {

    }
 }
```
