- [Introduction](#introduction)
- [Getting started](#getting-started)
- [Basic Routing](#basic-routing)
    + [Route Parameters](#route-parameters)
    + [Adding Query Vars](#adding-query-vars)
    + [Flushing Route Rules](#flushing-route-rules)
- [WordPress Conditions Routing](#wordpress-conditions-routing)
    + [Conditions](#conditions)
    + [Filters](#filters)


<a name="introduction"></a>
## [Introduction](#introduction)

The [WordPress Template Hierarchy](https://developer.wordpress.org/files/2014/10/template-hierarchy.png) is not the most intuitive thing. Because of that Assely routing flattens this hierarchy and immediately fallbacks to the `index.php`, where the router is executing.

<a name="getting-started"></a>
## [Getting started](#getting-started)

All application routes should be defined inside `app\Http\routes.php` file. This file is included on application bootstrap by the `App\Providers\HttpServiceProvider` class.

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

<a name="basic-routing"></a>
## [Basic Routing](#basic-routing)

[alert type="info"]Basic Routing relies on [Rewrite API](https://codex.wordpress.org/Rewrite_API/add_rewrite_rule) and closes all of this messy code into one readable API. However, it's recommended to know how it is working.[/alert]

[alert type="warning"]You have to take into account that plugins that heavy relies on WordPress default query may sometimes not work properly. For example, SEO may have hard time to setup valid meta tags for advanced custom routes.[/alert]

Adding routes is pretty straightforward, just give URL to which route has to answer. Routes are registered using [add_rewrite_rule](https://codex.wordpress.org/Rewrite_API/add_rewrite_rule).

```php
Route::get('/', $callback);

Route::get('movies/lastest', $callback);
```

<a name="route-parameters"></a>
### [Route Parameters](#route-parameters)

You can define route parameters for pulling values of specific URL segments. Parameters are registered using [add_rewrite_tag](https://codex.wordpress.org/Rewrite_API/add_rewrite_tag).

[alert type="info"]WordPress uses specific names for particular properties. For example, `name` for post slug or `paged` for current pagination number. It is better to use that names as your route parameters. Among others, helps with proper handling 'Not found' error.[/alert]

```php
Route::get('movie/{name}', function($name) {
    //
});

Route::get('movie/{name}/actor/{actor}', function($name, $actor) {
    //
});
```

As you can see, the parameters will be passed into your route's callback when it is resolved.

#### Forcing format of the parameters

You may enforce route parameters formats with `where` method. For example, id can be only numeric.

```php
Route::get('movie/{id}', function($id) {
    //
})->where(['id' => '([0-9]+)']);
```

By default, if parameters formats are not specifed, the `([^/]+)` format is used. In words, capture everything betwen forward slashes.

<a name="adding-query-vars"></a>
### [Adding Query Vars](#adding-query-vars)

You can set additional route query variables with `queries` method. It accepts array where keys are parameters names and values are parameters values.

[alert type="info"][Query vars](https://codex.wordpress.org/Glossary#Query_Variable) helps to populate the default query. All available query strings you can find in [Codex](https://codex.wordpress.org/WordPress_Query_Vars).[/alert]

As example, when particular route have to display list of post from `movies` custom post type, help WordPress understand that by providing `post_type=movie` query string:

```php
Route::get('movie/{name}', function($name) {
    //
})->queries(['post_type' => 'movie']);
```

Example above will add to the registered rewrite rule `post_type=movie` parameter. The route rewrite url will look like this `index.php?posttype=movie&name=$matches[1]`.

<a name="flushing-route-rules"></a>
### [Flushing Route Rules](#flushing-route-rules)

The easiest way to clear routes rewrite rules is to run `wp assely:clear rewrites` command. This will flush and refresh all rules.

However, rewrite rules can also be refreshed manually. Go into `Settings > Permalinks` and resave "Common Settings".

[alert type="danger"]Remember to clear rewrite rules every time you made changes to your application routes. Otherwise you can come up with weird errors.[/alert]

<a name="wordpress-conditions-routing"></a>
## [WordPress Conditions Routing](#wordpress-conditions-routing)

Condition Routing is based on [Conditional Tags](https://codex.wordpress.org/Conditional_Tags) and it's simple, but less powerfull alternative for standard routing.

This type of routes needs to be defined is special pattern `<condition>:<filter>`, where condition name and filter name are separated with colon.

```php
// Condition without filter
Route::get('front', $callback);

// Condition with filter
Route::get('post:1', $callback);
```

<a name="conditions"></a>
### [Conditions](#conditions)

Table below presents names of all handled conditions and which ones accepts filters. For more explanations refer to the [Codex](https://codex.wordpress.org/Conditional_Tags).

| Condition name    | Condition function    | Accepts filter    |
|-------------------|-----------------------|-------------------|
| 404               | is_404                | ✖                 |
| archive           | is_archive            | ✖                 |
| attachment        | is_attachment         | ✖                 |
| author            | is_author             | ✔                 |
| category          | is_category           | ✔                 |
| date              | is_date               | ✖                 |
| day               | is_day                | ✖                 |
| front             | is_front_page         | ✖                 |
| home              | is_home               | ✖                 |
| list              | is_post_type_archive  | ✔                 |
| month             | is_month              | ✖                 |
| page              | is_page               | ✔                 |
| paged             | is_paged              | ✖                 |
| post              | is_single             | ✔                 |
| search            | is_search             | ✖                 |
| single            | is_singular           | ✔                 |
| sticky            | is_sticky             | ✔                 |
| tag               | is_tag                | ✔                 |
| taxonomy          | is_tax                | ✔                 |
| time              | is_time               | ✖                 |
| year              | is_year               | ✖                 |

<a name="filters"></a>
### [Filters](#filters)

Filter can accept multiple values. Separate each with comma within "curly" braces `{}`. You can use ids, slugs, even titles.

Route bellow will match post with ids 1, 2 and slug `sample-page`.

```php
Route::get('post:{1,2,sample-page}', $callback);
```
