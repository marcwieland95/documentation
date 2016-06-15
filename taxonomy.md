- [Introduction](#introduction)
- [Creating taxonomy](#creating-taxonomy)
    + [Via console command](#via-console-command)
    + [Manually created file](#manually-created-file)
- [Configuring taxonomy](#configuring-taxonomy)
    + [Registering taxonomy](#registering-taxonomy)
    + [The Arguments method](#the-arguments-method)
    + [The Relation method](#the-relation-method)
    + [The Fields method](#the-fields-method)
    + [The Columns method](#the-columns-method)
- [Using taxonomy](#using-taxonomy)
    + [Quering terms](#quering-terms)
    + [Inserting terms](#inserting-terms)
    + [Updating terms](#updating-terms)
    + [Deleting terms](#deleting-terms)
    + [Rendering terms in templates](#rendering-terms-in-templates)


<a name="introduction"></a>
## [Introduction](#introduction)

[alert type="info"]Taxonomies are a part of singularities. I encourage you to read general [Singularities](/docs/singularities) documentation chapter.[/alert]

[Taxonomy](https://codex.wordpress.org/Taxonomies) allows to classify your posttypes in groups. The names for the different groupings in a taxonomy are called terms.

<a name="creating-taxonomy"></a>
## [Creating taxonomy](#creating-taxonomy)

By default, taxonomies are stored inside `app\Taxonomies` directory.

### Via console command

Scafford taxonomy with `wp assely:make taxonomy` command.

```bash
wp assely:make taxonomy Genres
```

##### Specifying slug

This will create taxonomy with `genres` slug, but you can specify it with `--slug` option.

```bash
wp assely:make taxonomy Genres --slug="genre"
```

##### Specifying owners

You can also define to which posttype this taxonomy belongs to. Enter it's classname to the `--belongsto` option.

```bash
wp assely:make taxonomy Genres --belongsto="App\Posttypes\Movies"
```

### Manually created class file

Make file with class that extends base `Assely\Taxonomy\Taxonomy` class.

The `slug` property is required. This value determines under which name your taxonomy will be registered and accessible.

```php
// app/Taxonomies/Genres.php

namespace App\Taxonomies;

use Assely\Taxonomy\Taxonomy;

class Genres extends Taxonomy
{
    /**
     * Taxonomy slug.
     *
     * @var string
     */
    public $slug = 'genres';

    /**
     * Describe taxonomy relationships.
     *
     * @return self
     */
    public function relation()
    {
        return $this->belongsTo([
            'App\Posttypes\Movies',
            'App\Posttypes\Books',
        ]);
    }

    /**
     * Taxonomy arguments.
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
     * Register taxonomy custom fields.
     *
     * @return \Assely\Field\Field[]
     */
    public function fields()
    {
        return [
            //
        ];
    }

    /**
     * Specify columns of the taxonomy terms list.
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

<a name="configuring-taxonomy"></a>
## [Configuring taxonomy](#configuring-taxonomy)

<a name="registering-taxonomy"></a>
### [Registering taxonomy](#registering-taxonomy)

All taxonomies are registered in the `config/app.php` configuration file. This file contains a `taxonomies` array where you can add your newly created custom taxonomies.

```php
'taxonomies' => [
    // ...other taxonomies

    App\Taxonomies\Genres::class,
],
```

<a name="the-arguments-method"></a>
### [The Arguments method](#the-arguments-method)

The `arguments` method must always return an array of taxonomy registration parameters. You can use all of the default [WordPress parameters](https://codex.wordpress.org/Function_Reference/register_taxonomy#Arguments), but there is also some "assely-specific".

```php
public function arguments()
{
    return [
        'title' => ['Grene', 'Grenes'],
        'description' => 'Movies grenes',
    ];
}
```

| Parameter name | Default value | Description |
|---------|---------|---------|
| title | `[]` | Array of titles, where first element is singular variant, second a plural |
| description | `''` | Description text displayed above all fields. |
| preserve | `'single'` | How metabox [preserves his metadata](/docs/singularities#configuring-how-metadata-is-stored). |

<a name="the-relation-method"></a>
### [The Relation method](#the-relation-method)

Taxonomies must belong to one or multiple posttypes. The `relation` method allows you to describe where taxonomy should be assigned. Use `belongsTo` method with array of post types class names as parameter.

```php
public function relation()
{
    return $this->belongsTo([
        'App\Posttypes\Movies',
        'App\Posttypes\Books',
    ]);
}
```

<a name="the-fields-method"></a>
### [The Fields method](#the-fields-method)

The `fields` method allows to list custom fields which will be managed by term.

[alert type="info"]More detailed descriptions about fields you can read in [Fielder documentation](/docs/field-types).[/alert]

```php
use Field;

public function fields()
{
    return [
        Field::colorpicker('color')
    ];
}
```

<a name="the-columns-method"></a>
### [The Columns method](#the-columns-method)

The `columns` method allows to define custom taxonomy columns which are displayed on the list of terms.

Taxonomies accepts one kind of column: term.

```php
public function columns()
{
    return [
        Column::term('App\Taxonomies\Genres', 'color'),
    ];
}
```

[alert type="info"]More accurate description about `Column` facade you can find in [column documentation](/docs/column).[/alert]

<a name="using-taxonomy"></a>
## [Using taxonomy](#using-taxonomy)

Injecting the taxonomy repository provides fluent access to it's terms. However, you can also use `Term` facade.

```php
Term::type('genres')->find(1);
```

<a name="quering-terms"></a>
### [Quering terms](#quering-terms)

Of course, once you created and registered your taxonomy, you can start retrieving terms for your views. In this section you will learn how you can query your custom taxonomies.

#### Fetching multiple terms

The `all` method will retrive all of the taxonomy terms. This method returns an instance of `Illuminate\Support\Collection`, so you can easily modify results with [diffrent methods](https://laravel.com/docs/5.2/collections#available-methods).

```php
use App\Taxonomies\Genres;

public function index(Genres $genres) {
    view('index', [
        'genres' => $genres->all(),
    ]);
}
```

#### Retrieving single post

You can also immediately search for single term. Pass term id as the `find` method argument.

```php
use App\Taxonomies\Genres;

public function index(Genres $genres) {
    view('show', [
        'genre' => $genres->find(1),
    ]);
}
```

##### Not Found Query Exception

Sometimes you want to throw an exception when term was not found. The `findOrFail` method returns found term, but if not `Assely\Database\QueryException` will be thrown.

```php
$genres->findOrFail(1);
```

#### Custom query

Needs something more complex? You can use `query` method with array of [ parameters](https://developer.wordpress.org/reference/functions/get_terms/) as argument.

```php
use App\Taxonomies\Genres;

public function index(Genres $genres) {
    view('index', [

        'mostUsedTerms' => $genres->query([
            'orderby' => 'count'
        ]),

    ]);
}
```

<a name="inserting-terms"></a>
### [Inserting terms](#inserting-terms)

Use `create` method with array of properties and values for inserting new post to the database.

```php
use App\Taxonomies\Genres;

public function index(Genres $movies) {
    $movies->create([
        'title' => 'Really scary horrors',
    ]);
}
```

##### Inserting Query Exception

The `createdOrFail` method will insert new genre, but when error occurs `Assely\Database\QueryException` will be thrown.

```php
$genres->createOrFail([
    'title' => 'Really scary horrors',
]);
```

<a name="updating-terms"></a>
### [Updating terms](#updating-terms)

To update a term, you need to set new property values on retrived `Assely\Adapter\Term` instance, and then call `save` method.

```php
use App\Taxonomies\Genres;

public function index(Genres $genres) {
    $genre = $genres->find(1);

    $genre->title = 'Really scary horrors';

    $genre->save();
}
```

<a name="deleting-terms"></a>
### [Deleting terms](#deleting-terms)

Call `destroy` method on retrieved `Assely\Adapter\Term` instance to remove term from database.

```php
use App\Taxonomies\Genres;

public function index(Genres $genres) {
    $genre = $genres->find(1);

    $genre->destroy();
}
```

<a name="rendering-terms-in-templates"></a>
### [Rendering terms in templates](#rendering-terms-in-templates)

You have access to bunch of diffrent properties. List of all you can find in [adapters documentation](/docs/adapters#term).

```html
<div class="genre">
    {{ $genre->title }} <span>{{ $genre->count }}</span>
</div>
```

#### Looping on terms collection

Easy as pie. Just standard loop.

```html
@foreach($genres->all() as $genre)
    <a href="{{ $genre->link }}">{{ $genre->title }}</a>
@endforeach
```

#### Accessing term metadata

Your terms may hold various metadata. The `meta` method helps you retrieve these informations. You only need to pass metadata key under which they are stored.

```html
{{ $genre->meta('color') }}
```

#### Retrieving term posts

To retrieve the posts with specific term, you can use `posts` method with taxonomy slug as argument.

```html
@foreach($genres->all() as $genre)
    @foreach($genre->posts('movies') as $post)
        <h1>{{ $post->title }}</h1>
    @endforeach
@endforeach
```
