- [Introduction](#introduction)
    + [Creating Pagination Routes](#creating-pagination-routes)
    + [Accessing page number in Controller](#accessing-page-number-in-controller)
    * [Paginating Query and Creating Pagination](#paginating-query-and-creating-pagination)
    + [Displaying Pagination](#displaying-pagination)


<a name="introduction"></a>
## [Introduction](#introduction)

Pagination allows you to split posts into multiple pages.

<a name="creating-pagination-routes"></a>
### [Creating Pagination Routes](#creating-pagination-routes)

First, inside `routes.php` create pagination route. **Remember to add `post_type` query parameter with slug of posttype that you will be paginating.** It's worth also to limit `paged` parameter to only numbers.

```php
Route::get('movies/page/{paged}', 'MoviesController@index')
    ->queries(['post_type' => 'movies'])
    ->where(['paged' => '([0-9]{1,})']);
```

[alert type="warning"]Better name pagination route properties as `paged`. WordPress use this term in its various functions and searches for it in URI query.[/alert]

<a name="accessing-page-number-in-controller"></a>
### [Accessing page number in Controller](#accessing-page-number-in-controller)

Now you can access `paged` parameter inside given controller method.

```php
namespace App\Http\Controllers;

class MoviesController extends Controller {
    /**
     * Show all application movies.
     */
    public function index($paged = 1)
    {
        // Pagination number
        dd($paged);
    }
}
```

<a name="paginating-query-and-creating-pagination"></a>
### [Paginating Query and Creating Pagination](#paginating-query-and-creating-pagination)

#### Paginating Posts before Quering

Next, you need to setup pagination before quering results. Use `paginate` method where and pass current page number as first argument.

By default, number of posts displayed per page are specifed by `get_option('posts_per_page')` option. However, if you uses diffrent number you can pass it as second argument.

```php
Post::type('movies')->paginate($paged, $perPage = null);
```

#### Creating Pagination Instance

Last thing, make pagination instance. Simply call `make` method with current page number as argument and pass it to the view.

```php
Pagination::make($paged, $perPage = null);
```

#### Example flow in Controller

```php
namespace App\Http\Controllers;

use View;
use App\Posttypes\Movies;
use Assely\Pagination\Pagination;

class MoviesController extends Controller {
    /**
     * Show all application movies.
     */
    public function index(
        Movies $movies,
        Pagination $pagination,
        $paged = 1
    ) {
        View::make('movies.index', [
            'movies' => $movies->paginate($paged)->all(),
            'pagination' => $pagination->make($paged),
        ]);
    }
}
```

<a name="displaying-pagination"></a>
### [Displaying Pagination](#displaying-pagination)

You are ready to display results of pagination inside your views. You have access to all nessecary elements on pagination and pagination items instances. Freedom in building pagination markup.

#### Pagination Properties

- `$pagination->currentPage` - Current pagination page number.
- `$pagination->next` - Next page pagination item.
- `$pagination->previous` - Previous page pagination item.
- `$pagination->items` - Collection of pagination items.

#### Pagination Item Properties

- `$item->title` - Item title.
- `$item->type` - Item type. Can be: number, dots, next, previous.
- `$item->number` - Item page number.
- `$item->link` - Item page link.
- `$item->active` - Indicator for currently active page.

#### Example Pagination View

```html
@foreach ($movies as $movie)
    <h1>{{ $movie->title }}</h1>
@endforeach

<ul>
    @if($pagination->previous)
        <li>
            <a href="{{ $pagination->previous->link }}">
                {{ $pagination->previous->title }}
            </a>
        </li>
    @endif

    @foreach($pagination->items as $item)
        <li>
            @if(! $item->active && $item->type !== 'dots')
                <a href="{{ $item->link }}">
                    {{ $item->title }}
                </a>
            @else
                <span>{{ $item->title }}</span>
            @endif
        </li>
    @endforeach

    @if($pagination->next)
        <li>
            <a href="{{ $pagination->next->link }}">
                {{ $pagination->next->title }}
            </a>
        </li>
    @endif
</ul>
```
