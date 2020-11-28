# Install Livewire with Laravel 8

### Installation
Install the package:
```bash
composer require livewire/livewire
```

Publishing the config file:
```bash
php artisan livewire:publish --config
```

Publishing frontend assets:
```bash
php artisan livewire:publish --assets
```

Paste this in `composer.json` file:
```json
...
{
    "scripts": {
        "post-autoload-dump": [
            "Illuminate\\Foundation\\ComposerScripts::postAutoloadDump",
            "@php artisan package:discover --ansi",
            "@php artisan vendor:publish --force --tag=livewire:assets --ansi"
        ]
    }
}
...
```

#### /resources/views/index.blade.php
```blade
<!DOCTYPE html>

<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <script src="https://cdn.jsdelivr.net/gh/alpinejs/alpine@v2.7.3/dist/alpine.min.js" defer></script>

    @livewireStyles

    <link rel="stylesheet" href="{{ mix('/assets/css/app.css') }}">

    <title>@yield('title', 'Home') - Template</title>
</head>

<body>

    @livewire('counter')

    <script src="{{ mix('/assets/js/app.js') }}"></script>

    @livewireScripts
    
</body>

</html>

```
