# Laravel API Helper

A set of API tools for Laravel projects.

# Installation

## Prerequisites

 - PHP7.1+
 - Laravel project

## Install via composer to the Laravel project

- Run composer command

`composer require mpokket/laravel-api-helper`

- Register the Service Provider

In `config/app.php`, add the service provider to `$providers` array

```php

'providers' => [
    ...
    Mpokket\APIHelper\APIHelperServiceProvider::class,
    ...
]
```

## APIHelperServiceProvider

APIHelperServiceProvider provides **Mpokket\APIHelper\Annotations\Deprecation** Annotation for your API methods which 
supports the following IETF RFCs,

- RFC8594 - **The Sunset HTTP Header Field**
- Internet Draft draft-dalal-deprecation-header-01 - **The Deprecation HTTP Header Field**


# Usage

To add [Sunset](https://datatracker.ietf.org/doc/html/rfc8594) and 
[Deprecation](https://tools.ietf.org/id/draft-dalal-deprecation-header-01.html) for your APIs, use the following 
annotations over the API method directly.

## Annotation options

```php
/**
* @Deprecation(since=true) // If you don't know the date the API will deprecated on
* Deprecation annotation attributes
* since - true or the date and time (optional)
* sunset - Date and time (optional)
* alternate - URL for superseding API
* policy - URL for sunset policy 
*/
```

### Example response header:

#### Example 1

Annotation

```php
use Mpokket\APIHelper\Annotations\Deprecation; // DO NOT FORGET TO IMPORT THE ANNOTATION

/**
* Display a listing of the resource.
*
* @Deprecation(since="true") // or
* @Deprecation(since=true) // or
* @Deprecation(since="01-01-2023")
*
* @return \Illuminate\Http\Response
*/
public function index()
{
    return response('...');
}
```

```yaml
Deprecation: true
```


#### Example 2

Annotation

```php

use Mpokket\APIHelper\Annotations\Deprecation; // DO NOT FORGET TO IMPORT THE ANNOTATION

/**
* Display a listing of the resource.
*
* @Deprecation(since="31-12-2022", alternate=" https://domain.com/your/next/version/api", 
*     policy="https://domain.com/api/deprecation/policy", sunset="01-01-2022")
*
* @return \Illuminate\Http\Response
*/
public function index()
{
    return response('...');
}
```

Response headers
```yaml
Sunset: Mon, 01 Jan 2022 00:00:00 GMT
Deprecation: Mon, 31 Dec 2022 00:00:00 GMT
Link: https://domain.com/your/next/version/api; rel=alternate, https://domain.com/api/deprecation/policy; rel=deprecation
```

# Notes
- [Deprecate](https://tools.ietf.org/id/draft-dalal-deprecation-header-01.html) header IETF proposal is currently in 
draft status
- PHPUnit tests are not done for testing deprecate middleware

# Debug

On non-production environment, if there's any exception from the middleware, there will be a response header that 
points to the exception and line number. Use this while reporting issues along with your controller method annotation.

# Support

If you require any support, kindly use GitHub issue tracker for this project.

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](README.md)
