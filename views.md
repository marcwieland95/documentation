- [Basic workflow](#basic-workflow)
    + [Create view template](#create-view-template)
    + [Render view inside controller](#render-view-inside-controller)
    + [Link Route with Controller](#link-route-with-controller)


<a name="basic-workflow"></a>
## [Basic workflow](#basic-workflow)

The Assely framework fully utilizes Laravel Views. Please visit official [documentation](https://laravel.com/docs/5.3/views) for a comprehensive descriptions.

Oh yes, you can use all that goodness in your Assely application.

<a name="create-view-template"></a>
### [Create view template](#create-view-template)

Make your view template inside `resources/views` directory.

```html
<!-- resources/views/welcome.blade.php -->

<html>
    <head>
        <title>Welcome to Assely</title>
    </head>
    <body>
        <h1>Hello, {{ $name }}!</h1>
    </body>
</html>
```

<a name="render-view-inside-controller"></a>
### [Render view inside controller](#render-view-inside-controller)

Create controller that renders your view.

```php
// app/Http/Controllers/WelcomeController.php

namespace App\Http\Controllers;

use View;

class HomeController extends Controller
{
    /**
     * Display a list of resources.
     *
     * @return void
     */
    public function index()
    {
        return View::make('welcome', ['name' => 'John Smith']);
    }
}
```

<a name="link-route-with-controller"></a>
### [Link Route with Controller](#link-route-with-controller)

```php
// app/Http/routes.php

Route::get('/', 'WelcomeController@index');
```

