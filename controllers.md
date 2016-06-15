- [Introduction](#introduction)
- [Basics of controllers](#basics-of-controllers)
- [Dependency Injection](#dependency-injection)
    - [Constructor Injection](#constructor-injection)
    - [Method Injection](#method-injection)
- [Controller Properties](#controller-properties)


<a name="introduction"></a>
## [Introduction](#introduction)

Controllers oranize your application logic into classes to which you can refer directly from routes and ajaxes. All controllers are stored inside `app\Http\Controllers` directory.

Each one controller should extend base `Assely\Routing\Controller` class in order to have access to addional properties like `Illuminate\Http\Request`.

<a name="basics-of-controller"></a>
## [Basics of controllers](#basics-of-controller)

First, you need to define route inside `routes.php` that points to the controller.

```php
Route::get('posttype:post', 'PostController@index');
```

On the route match, the `index` method will be called on `App\Http\Controllers\PostController` class.

Next, we need a controller itself. Make new file with choosen class name and within `App\Http\Controllers` namespace.

[alert type="info"]You can use `wp assely:make controller` commad to quickly scafford controllers.[/alert]

```php
<?php

namespace App\Http\Controllers;

use View;
use Post;

class PostController extends Controller
{
    /**
     * Display the specifed resource.
     *
     * @return void
     */
    public function show($id)
    {
        return View::make('post.single', ['post' => Post::find($id)]);
    }
}
```

<a name="dependency-Injection"></a>
## [Dependency Injection](#dependency-Injection)

<a name="constructor-injection"></a>
### [Constructor Injection](#constructor-injection)

Controllers are resolved by service container, so you are able to type-hint dependences and they will be automatically injected for you.

```php
<?php

namespace App\Http\Controllers;

use View;
use App\Posttypes\Posts;

class PostController extends Controller
{
    /**
     * The posts instance.
     *
     * @var \App\Posttypes\Posts
     */
    protected $posts;

    /**
     * Construct controller.
     *
     * @param \App\Posttypes\Posts
     */
    public function __construct(Posts $posts) {
        $this->posts = $posts;
    }
}
```

<a name="method-injection"></a>
### [Method Injection](#method-injection)

You also can type-hint controller methods dependeces.

[alert type="danger"]When controller method use both dependences and route properties, make sure that properties are after dependences. Otherwise service container will not be able to automatically resolve type-hinted dependences.[/alert]

```php
<?php

namespace App\Http\Controllers;

use View;
use App\Posttypes\Posts;
use App\Taxonomies\Categories;

class PostsController extends Controller
{
    /**
     * Display the specifed resource.
     *
     * @return void
     */
    public function show(
        Posts $post,
        Categories $categories,
        $categoryId
    ) {
        return View::make('posts.index', [
            'posts' => $posts->withTerm($categories->find($categoryId))
        ]);
    }
}
```

<a name="controller-properties"></a>
## [Controller Properties](#controller-properties)

##### Request

Inside controllers, which extends base controller, you have access to the request object.

```php
<?php

namespace App\Http\Controllers;

class IndexController extends Controller
{
    /**
     * Display a list of resources.
     *
     * @return void
     */
    public function index()
    {
        // Dump request data
        dd($this->request->getMethod());
    }
}
```
