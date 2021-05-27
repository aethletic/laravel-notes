# Custom Middleware

Custom middleware throttling by token.

```bash
php artisan make:middleware TokenThrottleMiddleware
```

```php
//app/Http/Middleware/TokenThrottleMiddleware.php

<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Illuminate\Routing\Middleware\ThrottleRequests;
use RuntimeException;

class TokenThrottleMiddleware extends ThrottleRequests
{
    protected function resolveRequestSignature($request)
    {
        if ($route = $request->route()) {
            return sha1(request('token', 'none'));
        }

        throw new RuntimeException('Unable to generate the request signature. Route unavailable.');
    }

}
```

```php
//app/Http/Kernel.php

protected $routeMiddleware = [
    // ...
    'token.throttle' => \App\Http\Middleware\TokenThrottleMiddleware::class,
    // ...
];
```

```php
// routes/api.php

Route::group(['prefix' => 'v1', 'middleware' => 'token.throttle:30,1'], function () {
  // some awesome shit
});
```

Source: https://bannister.me/blog/custom-throttle-middleware/
