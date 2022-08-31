# Generic PSR-15 Middleware & Handlers

![PHP from Packagist](https://img.shields.io/packagist/php-v/dakujem/generic-middleware)
[![Test Suite](https://github.com/dakujem/generic-middleware/actions/workflows/php-test.yml/badge.svg)](https://github.com/dakujem/generic-middleware/actions/workflows/php-test.yml)
[![Coverage Status](https://coveralls.io/repos/github/dakujem/generic-middleware/badge.svg?branch=main)](https://coveralls.io/github/dakujem/generic-middleware?branch=main)

> 💿 `composer require dakujem/generic-middleware`


## `GenericMiddleware`

The [`GenericMiddleware`] is a general purpose middleware that turns a callable into a PSR-15 implementation.
It accepts any callable with signature `fn(Request,Handler):Response`.

It can be used for convenient inline middleware implementation:
```php
$app->add(new GenericMiddleware(function(Request $request, Handler $next): Response {
    $request = $request->withAttribute('foo', 42);
    $response = $next->handle($request);
    return $response->withHeader('foo', 'bar');
}));
```


## `GenericHandler`

The [`GenericHandler`] is a general purpose request handler, as per the PSR-15 specification.
It turns a callable with signature `fn(Request):Response` into a PSR-15 implementation.

It can be used for convenient inline handler implementation
where you don't want to bother with neither anonymous classes nor named classes:
```php
$kernel = new GenericHandler(
    fn() => new Response(404, 'Not Found')
);
$dispatcher = new MiddlewareDispatcher($kernel);
```


>
> Note that I'm using aliases instead of full interface names in this documentation for brevity.
>
> Here are the full interface names:
>
> | Alias | Full class name |
> |:------|:----------------|
> | `Request` | `Psr\Http\Message\ServerRequestInterface` |
> | `Response` | `Psr\Http\Message\ResponseInterface` |
> | `Handler` | `Psr\Http\Server\RequestHandlerInterface` |
>


## Testing

Run unit tests using the following command:

`$` `composer test`


## Contributing

Ideas, feature requests and other contribution is welcome.
Please send a PR or create an issue.





[`GenericMiddleware`]: src/GenericMiddleware.php
[`GenericHandler`]: src/GenericHandler.php

