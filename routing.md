- [Introduction](#introduction)
- [Getting started](#getting-started)
- [Basics of Routing](#basics-of-routing)
    + [Route Parameters](#route-parameters)
    + [Routing Posttypes](#routing-posttypes)
    + [Routing Taxonomies](#routing-taxonomies)
    + [Routing Search Results](#routing-search-results)
    + [Routing Author Page](#routing-author-page)


<a name="introduction"></a>
## [Introduction](#introduction)

The [WordPress Template Hierarchy](https://developer.wordpress.org/files/2014/10/template-hierarchy.png) is not the most intuitive thing. Because of that Assely routing flattens this hierarchy and immediately fallbacks to the `index.php`, where the router is executing.

<a name="getting-started"></a>
## [Getting started](#getting-started)

All application routes should be defined inside `app\Http\routes.php` file. This file is included on application bootstrap by the `App\Providers\HttpServiceProvider` class.

<a name="basics-of-routin"></a>
## [Basics of Routing](#basics-of-routin)

Adding routes is pretty straightforward, just give URL to which route has to answer.

[alert type="warning"]Router respond only to registered and valid rewrite rules. Do not register any rewrite rules by itself.[/alert]

[alert type="info"]Remember! Routes registration order matters, only first matched route will be resolved. More specific ones should goes first, general last.[/alert]

```php
Route::get('/', $callback);

Route::get('movie', $callback);
```

#### HTTP Methods

You can register routes to respond for specified or multiple HTTP methods.

```php
Route::get($condition, $callback);
Route::post($condition, $callback);
Route::put($condition, $callback);
Route::delete($condition, $callback);

Route::match(['get', 'post'], $condition, $callback);
Route::any($condition, $callback);
```

<a name="route-parameters"></a>
### [Route Parameters](#route-parameters)

Route parameters allows to pull values of specific URL segments, which are reflected in WP_Query.

WordPress uses specific names for particular properties. For example, `name` for post slug or `paged` for current pagination number. You **have to** use that names as your route parameters.

[alert type="info"]All available query strings you can find in [Codex](https://codex.wordpress.org/WordPress_Query_Vars).[/alert]

```php
Route::get('movie/{name}', function($name) {
	$movie = Post::type('movie')->find($name);
});

Route::get('genre/{term}', function($term) {
	$term = Term::type('genre')->find($name);
});
```

As you can see, the parameter values are passed into your route's callback when it is resolved. You can use them to do further queries.

In the following part of this documentation you will find concrete examples how to correctly
 name your routes. Let's move on!

<a name="routing-posttypes"></a>
### [Routing posttypes](#routing-posttypes)

#### Single Post

```php
Route::get('{name}', function($name) {
	//
});
```

#### Paginated list of Posts

```php
Route::get('page/{paged}', function($paged) {
	//
});
```

#### Single Page

```php
Route::get('{pagename}', function($pagename) {
	//
});
```

#### Single custom type Post

```php
Route::get('movie/{name}', function($name) {
	//
});
```

#### Paginated list of custom type Posts

```php
Route::get('movie/page/{paged}', function($name, $paged) {
	//
});
```

<a name="routing-taxonomies"></a>
### [Routing taxonomies](#routing-taxonomies)

#### Single Category

```php
Route::get('category/{category_name}', function($category_name) {
	//
});
```

#### Single Tag

```php
Route::get('tag/{tag}', function($tag) {
	//
});
```

#### Single Term

```php
Route::get('genre/{term}', function($term) {
	//
});
```

#### Paginated Term

```php
Route::get('genre/{term}/page/{paged}', function($term, $paged) {
	//
});
```

<a name="routing-search-results"></a>
### [Routing Search Results](#routing-search-results)

```php
Route::get('search/{s}', function($s) {
	//
});
```

<a name="routing-author-page"></a>
### [Routing Author Page](#routing-author-page)

#### Single Author

```php
Route::get('author/{author_name}', function($author_name) {
	//
});
```

#### Paginated Author's Posts

```php
Route::get('author/{author_name}/page/{paged}', function($author_name, $paged) {
	//
});
```