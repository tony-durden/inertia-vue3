## Laravel Installation guide with Inertia.js & Vue.js 3
#### Create a Laravel project
````shell
composer create-project laravel/laravel <my-project-name>
````
#### Open laravel app directory
````shell
cd inertia-vue
````
#### Install Inertia.js as a dependency
````shell
composer require inertiajs/inertia-laravel
````

#### Setup Root Template
Next, setup the root template that will be loaded on the first page visit. This will be used to load your site assets (CSS and JavaScript), and will also contain a root `<div>` to boot your JavaScript application in.

rename root file: `/resources/app.blade.php`
````jsx
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
    <link href="{{ mix('/css/app.css') }}" rel="stylesheet" />
    <script src="{{ mix('/js/app.js') }}" defer></script>
    @inertiaHead
  </head>
  <body>
    @inertia
  </body>
</html>
````
#### Install middleware
````shell
php artisan inertia:middleware
````
Once generated, register the HandleInertiaRequests middleware in your `App\Http\Kernel` as the last item in your web middleware group.
````php
'web' => [
    // ...
    \App\Http\Middleware\HandleInertiaRequests::class,
],
````
## Install dependecies
### Install vue 3
````shell
npm install vue
````
### Install vue loader & compiler
````shell
npm install -D vue-loader vue-template-compiler
````
### Install the Inertia client-side adapters using NPM
````shell
npm install @inertiajs/inertia @inertiajs/inertia-vue3
````
### Progress indicator
````shell
npm install @inertiajs/inertia @inertiajs/inertia-vue3
````
## Initialize app.js
Directory: `/resources/js/app.js`
````js
import { InertiaProgress } from '@inertiajs/progress'
import { createApp, h } from 'vue'
import { createInertiaApp } from '@inertiajs/inertia-vue3'

InertiaProgress.init()

createInertiaApp({
  resolve: name => import(`./Pages/${name}`),
  setup({ el, App, props, plugin }) {
    createApp({ render: () => h(App, props) })
      .use(plugin)
      .mount(el)
  },
})
````
### Create Pages directory
add `Home.vue` inside `/resources/js/Pages/Home.vue`
```vue
<template>
    <div>
        <h1>Inertiajs is {{ test }}</h1>
        <Link href="/about-us">Go to about us</Link>
    </div>
</template>
<script>
import { Link } from "@inertiajs/inertia-vue3";
export default {
    components: {
        Link,
    },
    props: ["test"],
};
</script>
```
### Edit Webpack mix configuration
path: `webpack.mix.js`
```js
mix.js('resources/js/app.js', 'public/js')
    // Add vue
    .vue()
    .postCss('resources/css/app.css', 'public/css', [
        //
    ]);
```
## Create Valet link
```shell
valet link <mylink>
```

### Add vale lint to `webpack.mix.js`
```js
 mix.browserSync({
    proxy: "http://inertia-starter.test",
});
```
### Create routes
`/routes/web.php`

```php
use Illuminate\Support\Facades\Route;
use Inertia\Inertia;

Route::get('/', function () {
    return Inertia::render('Home', ['test' => 'working']);
});
```