# lumen-api-with-jwt

[Install Documentation Link Here](https://jwt-auth.readthedocs.io/en/develop/laravel-installation/)
## OR
#### "tymon/jwt-auth": "^1.0.0-rc.2"    composer.json
```json
    "require": {
        "php": ">=7.1.3",
        "fideloper/proxy": "~4.0",
        "laravel/framework": "5.6.*",
        "laravel/tinker": "~1.0",
        "tymon/jwt-auth": "^1.0.0-rc.2"
    },
```
##### Then update [composer update]




[Exception handling ](https://github.com/tymondesigns/jwt-auth/wiki/Authentication)

#### add this code in Exception/Handler.php

```php
    public function render($request, Exception $exception)
    {
        if($exception instanceof TokenInvalidException){
            return response()->json(['message'=>"Invalid Token Given !"], 400);
        }
        return parent::render($request, $exception);
    }
```




#### Create JWT Middleware
```php
     php artisan make:middleware JWT    

```

#### 1.Add this code into JWT class
```php
use JWTAuth;

class JWT
{


    public function handle($request, Closure $next)
    {
        JWTAuth::parseToken()->authenticate();
        return $next($request);
    }
}
```

#### 2. Register this into kernel

```php
    protected $routeMiddleware = [
        'auth' => \Illuminate\Auth\Middleware\Authenticate::class,
        'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
        'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
        'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
        'can' => \Illuminate\Auth\Middleware\Authorize::class,
        'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
        'jwt' => \App\Http\Middleware\JWT::class,
        'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
    ];
```

#### 3. add add in Authentication Controller
```php
    public function __construct()
    {
        $this->middleware('jwt', ['except' => ['login']]);
    }
```