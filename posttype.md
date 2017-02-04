- [Introduction](#introduction)
- [Creating posttype](#creating-posttype)
- [Configuring posttype](#creating-posttype)
    + [Registering posttype](#registering-posttype)
    + [The Arguments method](#the-arguments-method)
    + [The Columns method](#the-columns-method)
- [Using posttype](#using-posttype)
    + [Quering posts](#quering-posts)
    + [Inserting posts](#inserting-posts)
    + [Updating posts](#updating-posts)
    + [Deleting posts](#deleting-posts)
- [Displaying posts](#displaying-posts)
    + [Looping on collection of posts](#looping-on-collection-of-posts)
    + [Accessing post thumbnail](#accessing-post-thumbnail)
    + [Accessing post metadata](#accessing-post-metadata)
    + [Retrieving post terms](#retrieving-post-terms)
    + [Retrieving post comments](#retrieving-post-comments)


<a name="introduction"></a>
## [Introduction](#introduction)

[alert type="info"]Post types are a part of singularities. I encourage you to read general [Singularities](/docs/singularities) documentation chapter.[/alert]

[Posttype](https://codex.wordpress.org/Post_Types) refers to the various data that is maintained in the WordPress posts table. Custom post types allow developers to easily create and manage complex content structures.

<a name="creating-posttype"></a>
## [Creating posttype](#creating-posttype)

By default, post types are stored inside `app\Posttypes` directory.

#### Via console command

Scaffold posttype with `wp assely:make posttype` command.

```bash
wp assely:make posttype Movies
```

##### Specifying slug

This will create posttype with `movies` slug, but you can specify it with `--slug` option.

```bash
wp assely:make posttype Movies --slug="movie"
```

#### Manually created class file

Make file with class that extends base `Assely\Posttype\Posttype` class.

The `slug` property is required. This value determines under which name your posttype will be registered and accessible.

```php
// app/Posttypes/Movies.php

namespace App\Posttypes;

use Assely\Posttype\Posttype;

class Movies extends Posttype
{
    /**
     * Posttype slug.
     *
     * @var string
     */
    public $slug = 'movies';

    /**
     * Posttype arguments.
     *
     * @return array
     */
    public function arguments()
    {
        return [
            //
        ];
    }

    /**
     * Specify columns of the posttype posts list.
     *
     * @return \Assely\Column\Column[]
     */
    public function columns()
    {
        return [
            //
        ];
    }
}
```

<a name="configuring-posttype"></a>
## [Configuring posttype](#configuring-posttype)

<a name="registering-posttype"></a>
### [Registering posttype](#registering-posttype)

All posttypes are registered in the `config/singularities.php` configuration file. This file contains a `posttypes` array where you can add your newly created custom posttypes.

```php
'posttypes' => [
    // ...other posttypes

    App\Posttypes\Movies::class,
],
```

<a name="the-arguments-method"></a>
### [The Arguments method](#the-arguments-method)

The `arguments` method must always return an array of post type registration parameters. You can use all of the default [WordPress parameters](https://codex.wordpress.org/Function_Reference/register_post_type#Parameters), but there are also some "framework specific".

```php
public function arguments()
{
    return [
        'title' => ['Bollywood Movie', 'Bollywood Movies'],
    ];
}
```

| Parameter name | Default value | Description |
|---------|---------|---------|
| title | `[]` | Array of titles, where first element is singular variant, second a plural |

<a name="the-columns-method"></a>
### [The Columns method](#the-columns-method)

The `columns` method allows to define custom posttype columns which are displayed on the list of posts.

Posttypes accepts two kinds of columns: taxonomy and metabox.

```php
public function columns()
{
    return [
        Column::taxonomy('App\Taxonomies\Genres'),
        Column::metabox('App\Metaboxes\MovieDetails', 'release_date'),
    ];
}
```

[alert type="info"]More accurate description about `Column` facade you can find in [column documentation](/docs/column).[/alert]

<a name="using-posttype"></a>
## [Using posttype](#using-posttype)

Injecting the custom posttype repository provides fluent access to it's posts. However, you can also use `Post` facade.

```php
Post::type('movies')->find(1);
```

<a name="quering-posts"></a>
### [Quering posts](#quering-posts)

Of course, once you created and registered your Posttype, you can start retrieving posts data for your views. In this section you will learn how you can query your custom post types.

#### Fetching multiple posts

The `all` method will retrive all of the posttype posts. This method returns an instance of `Illuminate\Support\Collection`, so you can easily modify results with [diffrent methods](https://laravel.com/docs/5.2/collections#available-methods).

```php
use App\Posttypes\Movies;

public function index(Movies $movies) {
    $movies = $movies->all();
}
```

<a name="paginating-posts"></a>
### [Paginating posts](#paginating-posts)

Before quering posts you can declare if they have to be paginated. Use `paginate` method.

It requires current page number to be passed as method argument.

```php
use App\Posttypes\Movies;

public function index(Movies $movies, $paged = 1) {
    $movies = $movies->paginate($paged)->all();
}
```

[alert type="info"]Take a look on [Pagination](/docs/pagination) documentation for comprehensive guide about paginating in Assely framework.[/alert]

#### Retrieving single post

You can also immediately search for single post. Pass post id or slug as the `find` method argument.

```php
use App\Posttypes\Movies;

public function index(Movies $movies) {
    // ..with post id
    $movie = $movies->find(1);

    // ..or post slug
    $movie = $movies->find('pirates-of-the-caribbean');
}
```

##### Not Found Query Exception

Sometimes you want to throw an exception when post was not found. The `findOrFail` method returns found post, but if not `Assely\Database\QueryException` will be thrown.

```php
$movies->findOrFail(1);
```

#### Fetching posts with specifed term

You may need to retrieve only posts with specifed term. The `withTerm` method with desired `Assely\Adapter\Term` instance passed as argument will return only posts which have this term.

```php
use App\Posttypes\Movies;
use App\Taxonomy\Genre;

public function index(Movies $movies, Genre $genres) {
    $comedy = $genres->find('comedy');

    $comedyMovies = $movies->withTerm($comedy);
}
```

#### Custom query

Needs something more complex? You can use `query` method with array of [Query Parameters](https://codex.wordpress.org/Class_Reference/WP_Query#Parameters) as argument.

```php
use App\Posttypes\Movies;

public function index(Movies $movies) {
    $arguments = [
        'tax_query' => [
            'relation' => 'AND',
            [
                'taxonomy' => 'genre',
                'field' => 'slug',
                'terms' => ['fantasy'],
            ],
            [
                'taxonomy' => 'actor',
                'field' => 'slug',
                'terms' => ['johnny-depp'],
            ],
        ]
    ];

    $johnnyDeppFantasyMovies = $movies->query($arguments);
}
```

<a name="inserting-posts"></a>
### [Inserting posts](#inserting-posts)

Use `create` method with array of properties and values for inserting new post to the database.

```php
use App\Posttypes\Movies;

public function index(Movies $movies) {
    $movies->create([
        'title' => 'Pirates of the Caribbean',
    ]);
}
```

##### Inserting Query Exception

The `createdOrFail` method will insert new post, but when error occurs `Assely\Database\QueryException` will be thrown.

```php
$movies->createOrFail([
    'title' => 'Pirates of the Caribbean',
]);
```

<a name="updating-posts"></a>
### [Updating posts](#updating-posts)

To update a post, you need to set new property values on retrived `Assely\Adapter\Post` instance, and then call `save` method.

```php
use App\Posttypes\Movies;

public function index(Movies $movies) {
    $movie = $movies->find(1);

    $movie->title = 'Pirates of the Caribbean';

    $movie->save();
}
```

<a name="deleting-posts"></a>
### [Deleting posts](#deleting-posts)

Call `destroy` method on retrieved `Assely\Adapter\Post` instance to remove post from database.

```php
use App\Posttypes\Movies;

public function index(Movies $movies) {
    $movie = $movies->find(1);

    $movie->destroy();
}
```

<a name="displaying-posts"></a>
## [Displaying posts](#displaying-posts)

You have access to bunch of diffrent properties. List of all you can find in [adapters documentation](/docs/adapters#post-adaptee).

```html
<div class="movie">
    <h1>{{ $movie->title }}</h1>
    <time>{{ $movie->created_at }}</time>
    <p>{{ $movie->content }}</p>
</div>
```

<a name="looping-on-collection-of-posts"></a>
### [Looping on collection of posts](#looping-on-collection-of-posts)

Easy as pie. Just standard loop.

```html
@foreach($movies->all as $movie)
    <a href="{{ $movie->link }}">{{ $movie->title }}</a>
@endforeach
```

<a name="accessing-post-thumbnail"></a>
### [Accessing post thumbnail](#accessing-post-thumbnail)

You can access post thumbnail image with `thumbnail` property. By default, it contains image size configured in `config/images.php` file. This property returns `Assely\Thumbnail\Image` instance with various image informations.

```html
<img src="{{ $movie->thumbnail->link }}" alt="{{ $movie->thumbnail->title }}">
```

You may need to display thumbnail in diffrent size. Use `thumbnail` method with thumbnail size slug as argument.

For example, you have registered [custom thumbnail size](/docs/thumbnail#creating-thumbnail) named `hero`, you can access it with this name.

```html
<img src="{{ $movie->thumbnail('hero')->link }}" alt="{{ $movie->thumbnail->title }}">
```

##### Checking if post have thumbnail

Of course, before displaying image, you can check if post thumbnail is actually set.

```html
@if($movie->hasThumbnail)
    <!-- post have thumbnail -->
@endif
```

<a name="accessing-post-metadata"></a>
### [Accessing post metadata](#accessing-post-metadata)

Your posts may hold various metadata. The `meta` method helps you retrieve these informations. You only need to pass metadata key under which they are stored.

```html
{{ $movie->meta('movie_details') }}
```

It returns an `Illuminate\Support\Collection`, so you have flexible way to access it's values.

```html
{{ $movie->meta('movie_details')->get('release_date') }}
```

<a name="retrieving-post-terms"></a>
### [Retrieving post terms](#retrieving-post-terms)

#### Getting all terms assigned to the post

To retrieve all terms assigned to the post, you can use `terms` property.

```html
@foreach($movie->terms as $term)
    <span>{{ $term->title }}</span>
@endforeach
```

#### Getting terms of specific taxonomy assigned to the post

`terms` method with taxonomy slug as argument will retrieve only post terms from specific taxonomy.

```html
@foreach($movie->terms('genre') as $genre)
    <span>{{ $genre->title }}</span>
@endforeach
```

<a name="retrieving-post-comments"></a>
### [Retrieving post comments](#retrieving-post-comments)

You can also access collection of post comments.

```html
@foreach($movie->comments as $comment)
    <span>{{ $comment->author }}</span>
    <p>{{ $comment->content }}</p>
    <time>{{ $comment->created_at }}</time>
@endforeach
```
