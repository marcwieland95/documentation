- [Introduction](#introduction)
- [Getting started](#getting-started)
- [Basics of Routing](#basics-of-routing)
    + [Route Parameters](#route-parameters)
    + [Route Conditions](#route-conditions)
- [Creating Routes](#creating-routes)
	+ [Routing Homepage](#routing-homepage)
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

<a name="basics-of-routing"></a>
## [Basics of Routing](#basics-of-routing)

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

[alert type="info"]Basic Routing relies on [Rewrite API](https://codex.wordpress.org/Rewrite_API/add_rewrite_rule) and closes all of this messy code into one readable API. However, it's recommended to know how it is working.[/alert]

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

<a name="route-conditions"></a>
### [Route Conditions](#route-conditions)

Routes can be additional guarded with conditions. Conditioned routes will be resolved only when all its conditions meets. That way you can easily create routes for specific posts, pages etc.

[alert type="info"]All available conditions and its aliases you can find in [WordpressConditions](https://github.com/assely/framework/blob/master/src/Assely/Routing/WordpressConditions.php#L12) class.[/alert]

To set conditions on selected route, simply call `where` method with array of conditions as parameter.

```php
Route::get('{name}', function($name) {
	//
})->where([
	'post' => 'hello-world'
]);
```

[alert type="warning"]Caution! There are conditions that only work inside loop (e.g. `sticky`). They cannot be use as route counditions.[/alert]

Route above will be only resolved on post with `hello-world` slug. Take a note, you can pass multiple condition values in array `'post' => [2, 'sample-post', 'Hello World!']`. Its can be id, slug or even title.

```php
Route::get('{name}', function($name) {
	//
})->where([
	'post' => [2, 'sample-post', 'Hello World!']
]);
```

<a name="creating-routes"></a>
## [Creating Routes](#creating-routes)

As was said before, you have to use specific names for route properties. Router requires these names to property resolve its values from global WP_Query.

This section contains list of all available routes, which comes with WordPress and don't require to register any of custom rewrite rules.

<a name="routing-homepage"></a>
### [Routing Homepage](#routing-homepage)

Routing to the homepage is easy as pie:

```php
Route::get('/', function() {
	//
});
```

Your website uses a static page and you want a explicitly route? Narrow route from above with `front` condition.

```php
Route::get('/', function() {
	//
})->where([
    'front' => true
]);
```

<a name="routing-posttypes"></a>
### [Routing Posttypes](#routing-posttypes)

#### Single Post

The `{name}` parameter allows you to route to the view of single post.

```php
Route::get('{name}', function($name) {
	//
});
```

#### Single Page

Use `{pagename}` to target single pages.

```php
Route::get('{pagename}', function($pagename) {
	//
});
```

If you would like to target also children pages, add second route with simple regular expression before parameter.

```php
Route::get('(.+)/{pagename}', function($pagename) {
    //
});
```

Want to target all parent pages and children pages with one route? Just modify a little regular expression from above.

```php
Route::get('(.+\/)?{pagename}', function($pagename) {
    //
});
```

Now, everything before `{pagename}` is optional, so both pages and children pages will be matched.

#### Single custom type Post

```php
Route::get('movie/{name}', function($name) {
	//
});
```

#### Paginated list of Posts

```php
Route::get('page/{paged}', function($paged) {
	//
});
```

#### Paginated list of Custom Type Posts

```php
Route::get('movie/page/{paged}', function($name, $paged) {
	//
});
```

<a name="routing-taxonomies"></a>
### [Routing Taxonomies](#routing-taxonomies)

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