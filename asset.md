- [Basics of assets](#basics-of-assets)
    - [Adding asset](#adding-asset)
    - [Getting asset](#getting-asset)
    - [Loading asset](#loading-asset)
    - [Queuing asset](#queuing-asset)
    - [Asset status](#asset-status)
- [Dispach areas](#dispatch-areas)
- [Placement and media types](#placement-and-media-type)
- [Dependences](#dependences)
- [Localizing with data](#localizing-with-data)
- [Asyncing and defering](#asyncing-and-defering)
- [Arguments](#arguments)


<a name="basics-of-assets"></a>
## [Basics of assets](#basics-of-assets)

All application assets should be defined inside `app\Http\assets.php` file. This file is included on application bootstrap by the `App\Providers\HttpServiceProvider` class.

<a name="adding-asset"></a>
### [Adding asset](#adding-asset)

Providing application assets is really simple. Call `add` method on Asset facade and specify slug and file path in arguments. Asset type will be automatically detected by extension.

[alert type="info"]Asset added with relative path will be searched in `public` directory.[/alert]

```php
Asset::add('my-styles', ['path' => 'js/my-styles.css']);

Asset::add('my-script', ['path' => 'js/my-script.js']);
```

#### External asset

Adding assets directly form external resources (CDNs) is also supported.

```php
Asset::add('purecss', [
    'path' => 'https://cdnjs.cloudflare.com/ajax/libs/pure/0.6.0/pure.css'
]);

Asset::add('vuejs', [
    'path' => 'https://cdnjs.cloudflare.com/ajax/libs/vue/1.0.25/vue.min.js'
]);
```

<a name="getting-asset"></a>
### [Getting asset](#getting-asset)

You can pick previously registered asset with `get` method and it's slug as argument.

```php
Asset::get('my-script');
```

<a name="loading-asset"></a>
### [Loading asset](#loading-asset)

Sometimes you need to load asset that is already shiped with WordPress. To add this kind of assets you only need to enter it's slug and type.

```php
Asset::load('jquery', ['type' => 'script']);
```

<a name="queuing-asset"></a>
### [Queuing asset](#queuing-asset)

Assets can be queued. It basicaly register asset, but don't dispatch it immediately. This allows for adding asset only to specific views.

First queue asset inside `assets.php`.

```php
Asset::queue('my-script', ['path' => 'js/my-script.js']);
```

Then, in the selected view or controller, simply get desired asset and call `add` method.

```php
// ..somewhere in the blade template or controller.
Asset::get('my-script')->add();
```

<a name="asset-status"></a>
### [Asset status](#asset-status)

You are able to check asset status with `is` method.

Assets can have these statuses:
- `registered` - script was registered
- `enqueued` - script was enqueued
- `done` - script has been printed
- `to_do` - script has not yet been printed

```php
Asset::get('jquery')->is('enqueued');
```

<a name="dispatch-areas"></a>
## [Dispach areas](#dispatch-areas)

Assets can be add only to these areas: `website`, `admin` and `login`. By default, if area is not specifed, it will be a `website`.

```php
Asset::add('my-admin-script', [
    'path' => 'js/my-admin-script.js'
])->area('admin');
```

Area can be defined inside arguments too.

```php
Asset::add('my-login-script', [
    'path' => 'js/my-login-script.js',
    'area' => 'login',
]);
```

<a name="placement-and-media-types"></a>
## [Placement and media types](#placement-and-media-types)

#### For assets of script type

You can define where `<script>` markup of your asset should be printed. Scripts can be placed in `head` and `footer`. By default, it is `footer`.

```php
Asset::add('my-script', [
    'path' => 'js/my-script.js',
    'placement' => 'head'
]);
```

#### For assets of style type

Placement of styles reflects targeted devices. They can be added into diffrent media types. By default, it is `screen`.

All available values you can see in [W3C specification](http://www.w3.org/TR/CSS2/media.html#media-types).

```php
Asset::add('my-styles', [
    'path' => 'js/my-styles.css',
    'placement' => 'print'
]);
```

<a name="dependences"></a>
## [Dependences](#dependences)

Setting asset dependences will enforce assets to load in specified order. For example, if your script requires jQuery, set 'jquery' slug as your asset dependency. This will give you assurance that your asset will be loaded only when jQuery are ready.

[alert type="info"]Value of `dependences` argument should be an array.[/alert]

```php
Asset::add('my-script', [
    'path' => 'js/my-script.js',
    'dependences' => ['jquery']
]);
```

<a name="localizing-with-data"></a>
## [Localizing with data](#localizing-with-data)

Use `localize` method for localizing assets with some sort of data. It accepts two arguments. First is variable name under which the data will be available. Second is a callback, which should return an array.

[alert type="info"]You should add your own unique prefix to the variable name to avoid conflicts.[/alert]

```php
Asset::queue('my-script', [
    'path' => 'js/my-script.js'
])->localize('variable', function() {
    return ['data' => 'value']
});
```

This method use [wp_localize_script](https://codex.wordpress.org/Function_Reference/wp_localize_script) and outputs global javascript variable before asset markup tag.

```html
<script type="text/javascript">
/* <![CDATA[ */
    var variable = {
        'data': 'value'
    };
/* ]]> */
</script>

<script src="public/js/my-script.js"></script>
```

You can simply access this variable inside your javascript scripts.

```js
console.log(variable, window['variable']);
```

<a name="asyncing-and-defering"></a>
## [Asyncing and defering](#asyncing-and-defering)

Assets can be asynced or defered. You can setup these execution attributes with `async` and `defer` methods.

```php
// Execute asynchronously
Asset::add('my-script', [
    'path' => 'js/my-async-script.js'
])->async();

// Defer execution
Asset::add('my-script', [
    'path' => 'js/my-defer-script.js'
])->defer();
```

This methods simply adds to the `<script>` element special [HTML Attributes](https://developer.mozilla.org/pl/docs/Web/HTML/Element/script#Attributes). For example, above assets will output these markup.

```html
<script src="public/js/my-async-script.js" async></script>

<script src="public/js/my-defer-script.js" defer></script>
```

<a name="arguments"></a>
## [Arguments](#arguments)

Here is list of all asset arguments with short description.

| Argument | Default value | Descreption |
|---|---|---|
| path | `null` | Path to the asset file |
| area | `theme` | Asset destination area |
| type | `false` | Asset file type |
| media | `screen` | Asset media type |
| placement | `footer` | Asset html element placement |
| version | `null` | Asset version number |
| localize | `false` | Asset localization data |
| dependences | `[]` | List of asset dependences |
| execution | `[]` | List of asset exectution attributes |
