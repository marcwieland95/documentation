- [Introduction](#introduction)
- [Creating metabox](#creating-metabox)
    + [Via console command](#via-console-command)
    + [Manually created file](#manually-created-file)
- [Configuring metabox](#creating-metabox)
    + [Registering metabox](#registering-metabox)
    + [The Arguments method](#the-arguments-method)
    + [The Relation method](#the-relation-method)
    + [The Fields method](#the-fields-method)


<a name="introduction"></a>
## [Introduction](#introduction)

[alert type="info"]Metaboxes are a part of singularities. I encourage you to read general [Singularities](/docs/singularities) documentation chapter.[/alert]

Metaboxes bundles your custom fields in reusable modules, that you can assign to other singularities that support them.

<a name="creating-metabox"></a>
## [Creating metabox](#creating-metabox)

By default, metaboxes are stored inside `app\Metaboxes` directory.

### Via console command

Scaffold metabox with `wp assely:make metabox` command.

```bash
wp assely:make metabox MoviesDetails
```

##### Specifying slug

This will create metabox with `moviesdetails` slug, but you can specify it with `--slug` option.

```bash
wp assely:make metabox MoviesDetails --slug="movies-details"
```

##### Specifying owners

You can also define to which singularity this metabox belongs to. Enter it's classname to the `--belongsto` option.

```bash
wp assely:make metabox MoviesDetails --belongsto="App\Posttypes\Movies"
```

### Manually created class file

Make file with class that extends base `Assely\Metabox\Metabox` class.

The `slug` property is required. This value determines under which name your metadata will be registered and accessible.

```php
// app/Metaboxes/MovieDetails.php

namespace App\Metaboxes;

use Assely\Metabox\Metabox;

class MovieDetails extends Metabox
{
    /**
     * Metabox slug.
     *
     * @var string
     */
    public $slug = 'movie-details';

    /**
     * Describe metabox relationships.
     *
     * @return self
     */
    public function relation()
    {
        return $this->belongsTo([
            'App\Posttypes\Movies'
        ]);
    }

    /**
     * Metabox arguments.
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
     * Register metabox custom fields.
     *
     * @return \Assely\Field\Field[]
     */
    public function fields()
    {
        return [
            //
        ];
    }
}
```

<a name="configuring-metabox"></a>
## [Configuring metabox](#configuring-metabox)

<a name="registering-metabox"></a>
### [Registering metabox](#registering-metabox)

All metaboxes are registered in the `config/singularities.php` configuration file. This file contains a `metaboxes` array where you can add your newly created custom metaboxes.

```php
'metaboxs' => [
    // ...other metaboxes

    App\Metaboxes\MovieDetails::class,
],
```

<a name="the-arguments-method"></a>
### [The Arguments method](#the-arguments-method)

The `arguments` method must always return an array of metabox registration parameters.

```php
public function arguments()
{
    return [
        'title' => ['Movie Details', 'Movies Details'],
        'description' => 'Addtional movie informations.'
    ];
}
```

| Parameter name | Default value | Description |
|---------|---------|---------|
| title | `[]` | Array of titles, where first element is singular variant, second a plural. |
| description | `''` | Description text displayed above all fields. |
| location | `'normal'` | The location within the screen where the boxes should display. Can be `normal`, `side`, and `advanced`. |
| priority | `'default'` | Showing priority. Can be `default`, `low` and `height` |
| preserve | `'single'` | How metabox [preserves his metadata](/docs/singularities#configuring-how-metadata-is-stored). |

<a name="the-relation-method"></a>
### [The Relation method](#the-relation-method)

Metabox must belong to one or multiple singularities. The `relation` method allows you to describe where matabox should be assigned. Use `belongsTo` method with array of singularity classnames as parameter.

[alert type="info"]Metaboxes can be assigned to the Posts, Pages, Comments and all other registered Custom Posts.[/alert]

```php
public function relation()
{
    return $this->belongsTo(['App\Posttypes\Movies']);
}
```

Prefer to use name resolutions? No problem.

```php
use App\Posttypes\Movies;

public function relation()
{
    return $this->belongsTo([Movies::class]);
}
```

<a name="the-fields-method"></a>
### [The Fields method](#the-fields-method)

The `fields` method allows to list custom fields which will be managed by metabox.

[alert type="info"]More detailed descriptions about fields you can read in [Fielder documentation](/docs/fielder-usage).[/alert]

```php
use Field;

public function fields()
{
    return [
        Field::text('release_date')
    ];
}
```
