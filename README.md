# Reverser

[![Version](https://img.shields.io/packagist/v/aalfiann/reverser.svg)](https://packagist.org/packages/aalfiann/reverser)
[![Downloads](https://img.shields.io/packagist/dt/aalfiann/reverser.svg)](https://packagist.org/packages/aalfiann/reverser)
[![License](https://img.shields.io/packagist/l/aalfiann/reverser.svg)](https://github.com/aalfiann/reverser/blob/HEAD/LICENSE.md)

Reversing your actual backend behind PHP.

## Installation

Install this package via [Composer](https://getcomposer.org/).
```
composer require "aalfiann/reverser:^1.0"
```

## How to use
Create new file `index.php`
```php
use aalfiann\Reverser\Handler;
require_once ('vendor/autoload.php');

$handler = new Handler('https://yourbackend.com:8080');

// Prevents cURL from hanging on errors
$handler->setCurlOption(CURLOPT_CONNECTTIMEOUT, 1);
$handler->setCurlOption(CURLOPT_TIMEOUT, 5);

// This ignores HTTPS certificate verification, libcurl decided not to bundle ca certs anymore.
// Alternatively, specify CURLOPT_CAINFO, or CURLOPT_CAPATH if you have access to your cert(s)
$handler->setCurlOption(CURLOPT_SSL_VERIFYPEER, false);
$handler->setCurlOption(CURLOPT_SSL_VERIFYHOST, false);

// Check for a success
if ($handler->execute()) {
    //print_r($handler->getCurlInfo()); // Uncomment to see request info
} else {
    echo 'Oops, something went wrong!';
}

// close handler
$handler->close();
```
Example apache `.htaccess`
```
RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-l [NC]
RewriteCond %{REQUEST_FILENAME} !-f [NC]
RewriteCond %{REQUEST_FILENAME} !-d [NC]
RewriteCond %{REQUEST_URI} !^/index.php
RewriteRule ^(.+)$ index.php/$1 [QSA]
```

## Note
- Not strong in high traffic because this package doesn't included with **Cache**.
- HTMLProxyHandler class is require **"https"** protocol enabled in Libcurl.