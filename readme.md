# CORS Middleware for Laravel >= 5/6

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![Build Status][ico-travis]][link-travis]
[![Total Downloads][ico-downloads]][link-downloads]

Based on https://github.com/asm89/stack-cors

## What is CORS ?

CORS adalah mekanisme untuk memberi tahu browser, apakah sebuah request yang di-dispatch dari aplikasi web domain lain atau origin lain, ke aplikasi web kita itu diperbolehkan atau tidak. Jika aplikasi kita tidak mengijinkan maka akan muncul error, dan request pasti digagalkan oleh browser.

CORS hanya berlaku pada request-request yang dilakukan lewat browser, dari javascript; dan tujuan request-nya berbeda domain/origin. Jadi request yang dilakukan dari curl maupun dari back end, tidak terkena dampak aturan CORS.

## About

`Laravel-cors` package ini mengizinkan pengiriman [Cross-Origin Resource Sharing](http://enable-cors.org/)
header dengan konfigurasi di middleware Laravel.

Kamu bisa mempelajari penggunaannya secara global dalam sebuah wokflow dengan klik gambar di bawah ini.
this [image](http://www.html5rocks.com/static/images/cors_server_flowchart.png).

## Features

* Menghandel CORS pre-flight OPTIONS dalam sebuah request.
* Menambahkan CORS headers pada tiap respon.

## Installation

Require `barryvdh/laravel-cors` package dalam file `composer.json` dan update dependensi mu lewat terminal:
```sh
$ composer require barryvdh/laravel-cors
```

Untuk laravel >=5.5 keatas cukup sampai disini. Package ini juga support untuk Laravel yang lebih baru [Package Discovery](https://laravel.com/docs/5.5/packages#package-discovery).

Jika kamu menggunakan Laravel < 5.5, kamu perlu install v0.11.0:
```
composer require barryvdh/laravel-cors:0.11.0
```
Kamu juga perlu menambahkan Cors\ServiceProvider pada file `config/app.php` di array providers:
```php
Barryvdh\Cors\ServiceProvider::class,
```

## Global usage

Untuk menggunakan CORS di semua route, tambahkan `HandleCors` middleware dalam variabel `$middleware` property dari class  `app/Http/Kernel.php` :

```php
protected $middleware = [
    // ...
    \Barryvdh\Cors\HandleCors::class,
];
```

## Group middleware

Jika kamu hanya ingin menggunakan  CORS untuk middleware yang lebih spesifik dalam group atau route, tambahkan `HandleCors` ke dalam route middleware group:

```php
protected $middlewareGroups = [
    'web' => [
       // ...
    ],

    'api' => [
        // ...
        \Barryvdh\Cors\HandleCors::class,
    ],
];
```

## Configuration

Secara default cors bekerja pada `config/cors.php`. File `cors.php` akan ada secara otomatis saat kamu mem-publish ulang composer/vendor. Kamu dapat mem-publish config-nya dengan menjalankan command:
```sh
$ php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider"
```
> **Note:** Saat menggunakan custom headers, seperti `X-Auth-Token` atau `X-Requested-With`, kamu bisa setting itu dalam `allowedHeaders` untuk menambahkannya dalam headers. Kamu juga bisa menyetting nya dengan array `array('*')` untuk menambahkan semua custom headers.

    
```php
return [
     /*
     |--------------------------------------------------------------------------
     | Laravel CORS
     |--------------------------------------------------------------------------
     |
     | allowedOrigins, allowedHeaders and allowedMethods can be set to array('*')
     | to accept any value.
     |
     */
    'supportsCredentials' => false,
    'allowedOrigins' => ['*'],
    'allowedHeaders' => ['Content-Type', 'X-Requested-With'],
    'allowedMethods' => ['*'], // ex: ['GET', 'POST', 'PUT',  'DELETE']
    'exposedHeaders' => [],
    'maxAge' => 0,
];
```

`allowedOrigins`, `allowedHeaders` and `allowedMethods` can be set to `array('*')` to accept any value.

**Catatan Tambahan :**

> **Note:** If you are explicitly whitelisting headers, you must include `Origin` or requests will fail to be recognized as CORS.

> **Note:** Try to be a specific as possible. You can start developing with loose constraints, but it's better to be as strict as possible!

> **Note:** Because of [http method overriding](http://symfony.com/doc/current/reference/configuration/framework.html#http-method-override) in Laravel, allowing POST methods will also enable the API users to perform PUT and DELETE requests as well.


### Disabling CSRF protection for your API

Jika memungkinkan, gunakkan route group yang berbeda dengan CSRF protection enabled. 
Atau kamu juga bisa men-disable CSRF token (dan ini yang lebih mudah) untuk requests tertentu di class `App\Http\Middleware\VerifyCsrfToken`. Sebagai contoh untuk men-disable request untuk semua api, bisa menambahkan `'api/*'`:

```php
protected $except = [
    'api/*'
];
```
    
## License

Released under the MIT License, see [LICENSE](LICENSE).

[ico-version]: https://img.shields.io/packagist/v/barryvdh/laravel-cors.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/barryvdh/laravel-cors/master.svg?style=flat-square
[ico-scrutinizer]: https://img.shields.io/scrutinizer/coverage/g/barryvdh/laravel-cors.svg?style=flat-square
[ico-code-quality]: https://img.shields.io/scrutinizer/g/barryvdh/laravel-cors.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/barryvdh/laravel-cors.svg?style=flat-square

[link-packagist]: https://packagist.org/packages/barryvdh/laravel-cors
[link-travis]: https://travis-ci.org/barryvdh/laravel-cors
[link-scrutinizer]: https://scrutinizer-ci.com/g/barryvdh/laravel-cors/code-structure
[link-code-quality]: https://scrutinizer-ci.com/g/barryvdh/laravel-cors
[link-downloads]: https://packagist.org/packages/barryvdh/laravel-cors
[link-author]: https://github.com/barryvdh
[link-contributors]: ../../contributors
