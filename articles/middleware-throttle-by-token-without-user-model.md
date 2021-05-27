# Custom Middleware

Custom middleware throttling by token.

```bash
php artisan make:middleware TokenThrottleMiddleware
```

```php
//app/Http/Middleware/TokenThrottleMiddleware.php

<?php

namespace App\Http\Middleware;

use App\Models\Token;
use Closure;
use Illuminate\Http\Request;
use Illuminate\Routing\Middleware\ThrottleRequests;
use Illuminate\Support\Str;
use RuntimeException;

class TokenThrottleMiddleware extends ThrottleRequests
{
    /**
     * Resolve request signature.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return string
     *
     * @throws \RuntimeException
     */
    protected function resolveRequestSignature($request)
    {
        if ($route = $request->route()) {
            return sha1(request('token', 'none'));
        }

        throw new RuntimeException('Unable to generate the request signature. Route unavailable.');
    }

    /**
     * This OPTIONAL method for extend. 
     * You need install migration before use it.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int|string  $maxAttempts
     * @return int
     */
    protected function resolveMaxAttempts($request, $maxAttempts)
    {
        if (Str::contains($maxAttempts, '|')) {
            $maxAttempts = explode('|', $maxAttempts, 2)[$request->user() ? 1 : 0];
        }

        if (!is_numeric($maxAttempts)) {
            $token = Token::find(request('token'));
            $maxAttempts = $token->{$maxAttempts} ?? 0;
        }

        return (int) $maxAttempts;
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

Route::group(['prefix' => 'v1', 'middleware' => 'token.throttle:rate_limit,1'], function () {
  // some awesome shit
});
```

```php
// migration if we use dynamic throttle value

<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateTokensTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('tokens', function (Blueprint $table) {
            $table->id();
            $table->string('token', 64)->unique()->index();
            $table->smallInteger('rate_limit')->default(60);
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('tokens');
    }
}
```

```php
// token model

<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Token extends Model
{
    use HasFactory;

    protected  $primaryKey = 'token';

    protected $fillable = [
        'token',
        'rate_limit',
    ];
}
```

Source: https://bannister.me/blog/custom-throttle-middleware/
