- [Introduction](#introduction)
- [Basics of sidebars](#basic-sidebars)
    + [Creating sidebar](#creating-sidebar)
    + [Rendering sidebar](#rendering-sidebar)
    + [Checking if sidebar is used](#checking-if-sidebar-is-used)


<a name="introduction"></a>
## [Introduction](#introduction)

The WordPress [Sidebars](https://codex.wordpress.org/Sidebars) are areas where you may allocate customizable widgets. Next, inside your application templates, you can display all widgets assigned to the specified sidebar.

Sidebars creates easily customizable areas, which can be managed inside `Apperance > Widgets` administration panel.

<a name="basics-of-sidebars"></a>
## [Basics of sidebars](#basics-of-sidebars)

All application sidebars should be defined inside `app\Support\sidebars.php` file. This file is included on application bootstrap by the `App\Providers\AppServiceProvider` class.

<a name="creating-sidebar"></a>
### [Creating sidebar](#creating-sidebar)

Registering sidebars are really simple. All you have to do is to call `create` method on `Sidebar` facade.

```php
Sidebar::create('primary');
```

##### Arguments

Create method accepts options as second argument.

```php
Sidebar::create('primary', [
    'title' => ['Footer', 'Footers'],
    'description' => 'The website footer sidebar',
]);
```

List of available options:

| Option name | Default value | Description |
|---|---|---|
| title | `[]` | Area title displayed in the admin |
| description | `''` | Area description displayed in the admin |
| before_widget | `<div>` | Markup before widget |
| after_widget | `</div>` | Markup after widget |
| before_title | `<h2>` | Markup before sidebar title |
| after_title | `</h2>` | Markup after sidebar title |

<a name="rendering-sidebar"></a>
### [Rendering sidebar](#rendering-sidebar)

To render sidebar content just call `render` on previously picked sidebar.

```html
<div class="sidebar--primary">
    {{ Sidebar::get('primary')->render() }}
</div>
```

<a name="checking-if-sidebar-is-used"></a>
### [Checking if sidebar is used](#checking-if-sidebar-is-used)

You may want to display sidebar only if contains any widgets. Before rendering sidebar check it's status with `isActive` method.

```html
@if(Sidebar::isActive('primary'))
    <div class="sidebar--primary">
        {{ Sidebar::get('primary')->render() }}
    </div>
@endif
```
