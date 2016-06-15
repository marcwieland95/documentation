- [Introduction](#introduction)
- [Basics of menus](#basic-routing)
    + [Creating menu](#creating-menu)
    + [Getting menu](#getting-menu)
    + [Checking if menu is in use](#checking-if-menu-is-in-use)
    + [Rendering menu in templates](#rendering-menu-in-templates)


<a name="introduction"></a>
## [Introduction](#introduction)

The [Menus](https://codex.wordpress.org/Navigation_Menus) are lists of links that appear on your application.

Registering menus allow for creating easily customizable menus, which can be managed inside `Apperance > Menus` administration panel.

<a name="basics-of-menus"></a>
## [Basics of menus](#basics-of-menus)

All application menus should be defined inside `app\Support\menus.php` file. This file is included on application bootstrap by the `App\Providers\AppServiceProvider` class.

<a name="creating-menu"></a>
### [Creating menu](#creating-menu)

Registering menus are really simple. All you have to do is to call `create` method on `Menu` facade.

```php
Menu::create('primary');
```

##### Arguments

Create method accepts options as second argument.

```php
Menu::create('primary', [
    'title' => ['Main menu'],
]);
```

List of available options:

| Option name | Default value | Description |
|---|---|---|
| title | `[]` | Menu title displayed in the admin |

<a name="getting-menu"></a>
### [Getting menu](#getting-menu)

Calling `get` method will return previously registered menu instance.

```php
Menu::get('primary');
```

<a name="checking-if-menu-is-in-use"></a>
### [Checking if menu is in use](#checking-if-menu-is-in-use)

You can check if menu contains any items with `isActive` method.

```php
@if(Menu::get('primary')->isActive())
    // ...menu have items
@endif
```

<a name="rendering-menu-in-templates"></a>
### [Rendering menu in templates](#rendering-menu-in-templates)

Grab all menu items with `items` method. This will return collection of `Assely\Adapter\Menu` instances.

```html
<!-- resources/views/index.blade.php -->

<div class="menu--primary">
    @include('menu.nav', [
        'items' => Menu::get('primary')->items()
    ])
</div>
```

Now, you only need to loop through items in foreach statement.

```html
<!-- resources/views/menu/nav.blade.php -->

<ul>
    @foreach($items as $item)
        <li>
            <a href="{{ $item->link }}">{{ $item->title }}</a>

            <!--
            If menu item have children items,
            recursively include this view.
            -->
            @if($item->hasChildren())
                @include('menu.nav', [
                    'items' => $item->children()
                ])
            @endif
        </li>
    @endforeach
</ul>
```
