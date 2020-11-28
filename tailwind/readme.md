
# Install Tailwind with Laravel 8

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

### /resources/views/index.blade.php
```blade
<!DOCTYPE html>

<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" href="{{ mix('/assets/css/app.css') }}">

    <title>@yield('title') - Template</title>
</head>
    
<body>
        
    {{-- Code here... --}}
   
    <script src="{{ mix('/assets/js/app.js') }}"></script>
</body>

</html>
```
