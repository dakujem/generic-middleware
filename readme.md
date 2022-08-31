# Generic PSR-15 Middleware & Handlers

![PHP from Packagist](https://img.shields.io/packagist/php-v/dakujem/generic-middleware)
[![Test Suite](https://github.com/dakujem/generic-middleware/actions/workflows/php-test.yml/badge.svg)](https://github.com/dakujem/generic-middleware/actions/workflows/php-test.yml)
[![Coverage Status](https://coveralls.io/repos/github/dakujem/generic-middleware/badge.svg?branch=main)](https://coveralls.io/github/dakujem/generic-middleware?branch=main)

> 💿 `composer require dakujem/generic-middleware`

Contains a couple of classes:
- `GenericMiddleware`, an implementation of `Psr\Http\Server\MiddlewareInterface` 
- `GenericHandler`, an implementation of `Psr\Http\Server\RequestHandlerInterface` 

>
> Note that I'm using aliases `Request`, `Response` and `Handler` for their respective PSR interface names for brevity.
>
> Consider the following `use` statements in use:
> ```php
> use Psr\Http\Message\ServerRequestInterface  as Request;
> use Psr\Http\Message\ResponseInterface       as Response;
> use Psr\Http\Server\RequestHandlerInterface  as Handler;
> ```
>


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


## Testing

Run unit tests using the following command:

`$` `composer test`


## Contributing

Ideas, feature requests and other contribution is welcome.
Please send a PR or create an issue.





[`GenericMiddleware`]: src/GenericMiddleware.php
[`GenericHandler`]: src/GenericHandler.php

