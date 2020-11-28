# Install Livewire with Laravel 8

Install the package.
```bash
composer require livewire/livewire
```

Publishing the config file.
```bash
php artisan livewire:publish --config
```

Publishing frontend assets.
```bash
php artisan livewire:publish --assets
```

`composer.json`
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

