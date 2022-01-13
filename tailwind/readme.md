
# Install Tailwind with Laravel 8

## HOT Fix 01.12.2020 
Actual if `laravel-mix` version **<** `6.0.0`.

Replace to:
```json
"laravel-mix": "^6.0.0-beta.14",
```

## Install Tailwindcss
First, let's install NPM dependencies.
```bash
npm install
```

Now install tailwindcss via npm.
```bash
npm install tailwindcss
```

Create your Tailwind config file.
```bash
npx tailwindcss init
```

Finish by compiling your assets and you'll be ready.
```bash
npm run dev
```

Or for purge unused css code.
```
npm run prod
```

## Files

#### /resources/css/app.css
```css
html {
    font-family: "Rubik", sans-serif !important;
}

[x-cloak] {
    display: none !important;
}

@tailwind base;
@tailwind components;
@tailwind utilities;
@tailwind screens;

@layer base {
    
}

@layer components {
    
}

@layer utilities {
    
}

@layer screens {
    
}
```

#### /resources/sass/app.css
```css
html {
    font-family: "Rubik", sans-serif !important;
}

[x-cloak] {
    display: none !important;
}

@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
@import "tailwindcss/screens";

@layer base {
    
}

@layer components {
    
}

@layer utilities {
    
}

@layer screens {
    
}
```

#### /tailwind.config.js
```javascript
module.exports = {
    purge: [
        './resources/views/**/*.php'
    ],
    darkMode: false, // or 'media' or 'class'
    theme: {
        extend: {},
    },
    variants: {
        extend: {},
    },
    plugins: [],
}
```

#### /webpack.mix.js
You don't have to install autoprefixer or postcss-import, because it's already installed with laravel mix
```javascript
const mix = require('laravel-mix');

// js
mix.js('resources/js/app.js', 'public/assets/js')

// css
mix.postCss('resources/css/app.css', 'public/assets/css', [
        require('tailwindcss'),
    ]);

// scss
mix.sass('resources/sass/app.scss', 'public/assets/css')
    .options({
        processCssUrls: false,
        postCss: [
            require('tailwindcss'),
        ],
    });

if (mix.inProduction()) {
    mix.version();
} else {
    mix.sourceMaps();
}

```

#### /resources/views/index.blade.php
```blade
<!DOCTYPE html>

<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <meta name="csrf-token" content="{{ csrf_token() }}">

    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Rubik:wght@400;500;600;700;800;900&display=swap"
        rel="stylesheet">

    <link rel="stylesheet" href="{{ mix('/assets/css/app.css') }}">

    <title>@yield('title', 'Home') - {{ config('app.name') }}</title>

    @yield('styles')
</head>

<body>
    @yield('content')

    <script src="{{ mix('/assets/js/app.js') }}"></script>

    @yield('scripts')
</body>

</html>
```


```blade
@extends('layouts.app')

@section('content')
    <div class="flex flex-col h-screen">
        <header class="sticky top-0 h-16 border-b bg-white shrink-0 backdrop-blur bg-opacity-30 z-50">
            <div class="flex justify-between items-center h-full">
                {{-- start: logo --}}
                <div class="w-96">
                    <div class="px-8">
                        <div class="font-black text-lg uppercase">
                            Пульс
                        </div>
                    </div>
                </div>
                {{-- start: logo --}}

                {{-- start: navbar --}}
                <div class="flex justify-between flex-1 px-8">
                    {{-- start: navbar left --}}
                    <div>
                        left
                    </div>
                    {{-- end: navbar left --}}

                    {{-- start: navbar right --}}
                    <div>
                        right
                    </div>
                    {{-- end: navbar right --}}
                </div>
                {{-- end: navbar --}}
            </div>
        </header>
        <div class="flex-1 overflow-hidden">
            <div class="flex justify-between h-full">

                {{-- start: left-side --}}
                <aside class="h-full">
                    <div class="w-96 border-r h-full">
                        <div class="sticky top-0 p-8">
                            left side
                        </div>
                    </div>
                </aside>
                {{-- end: left-side --}}

                {{-- start: content --}}
                <main class="w-full py-8 overflow-y-auto">
                    <div class="max-w-3xl mx-auto">
                        @for ($i = 0; $i < 100; $i++)
                            content<br>
                        @endfor
                    </div>
                </main>
                {{-- end: content --}}

                {{-- start: right-side --}}
                <aside class="h-full">
                    <div class="w-96 border-l h-full">
                        <div class="sticky top-0 p-8">
                            right side
                        </div>
                    </div>
                </aside>
                {{-- end: right-side --}}

            </div>
        </div>
    </div>
@endsection
```
