- [Introduction](#introduction)
- [Creating Singularities](#creating-singularities)
    + [Via console command](#via-console-command)
    + [Manually created file](#manually-created-file)
- [Registering Singularities](#registering-singularities)
- [Dependency injection](#dependency-injection)
- [Configuring how metadata is stored](#configuring-how-metadata-is-stored)
    + [Preserve as single record](#preserve-as-single-record)
    + [Preserve as multiple records](#preserve-as-multiple-records)


<a name="introduction"></a>
## [Introduction](#introduction)

Singularities, this is how are named WordPress types of content in Assely framework. All of this types are grouped and unifed to common API. This way creating and managing your post types, taxonomies, metaboxes etc. is simple and intuitive. In this chapter you will learn how to create, register and use it inside your application.

<a name="creating-singularities"></a>
## [Creating Singularities](#creating-singularities)

By default, each application singularity are stored inside own type-named directory. For example, post types can be found in `app\Posttypes` folder, taxonomies inside `app\Taxonomies`, and so on.

[alert type="info"]This organization is not required, as long as singularities are correctly registered and autoloaded by Composer.[/alert]

### Via console command

Assely's WP-CLI commands can scafford singularities for you. Just type inside console `wp help assely:make` to see all available commands. List of all available you will find in [WP-CLI documentation](/docs/wp-cli).

```bash
wp assely:make posttype Movies

wp assely:make taxonomy Genres --belongsto="App\Posttypes\Movies"
```

### Manually created class file

When you do not have access to the WP-CLI, you need to create singularity manually. Make class that is extending base singularity and you are ready to go.

```php
namespace App\Posttypes;

use Assely\Posttype\Posttype;

class Movie extends Posttype {
    //
}
```

<a name="registering-singularities"></a>
## [Registering Singularities](#registering-singularities)

In order to register new singularities you need to add them inside `config/singularities.php` file. Each one have separated entry inside this file. Add singularity classname to the correct array. On start arrays already contains default singularities which are shiped with WordPress.

For example, you [created new post type](/docs/posttype#create) named `App\Posttypes\Movies`. To bootstrap this singularity, simply add his classname to the `posttypes` array:

```php
'posttypes' => [
    // ...other posttypes

    App\Posttypes\Movies::class,
],
```

<a name="dependency-injection"></a>
## [Dependency injection](#dependency-injection)

Type-hinted constructor parameters will be automatically resolved from the container. This is the best place to inject other application classes that may be used by singularity. For example, common usage is to inject post type for [Fielder selective field](/docs/fielder/types#selective).

```php
<?php

use Field;
use App\Posttypes\Movies;
use Assely\Profile\Profile;

class Reviewer extends Profile
{
    /**
     * Profile slug.
     *
     * @var string
     */
    public $slug = 'reviewer';

    /**
     * Construct profile.
     *
     * @param \App\Posttypes\Movies $movies
     */
    public function __construct(Movies $movies) {
        $this->movies = $movies;
    }

    /**
     * Profile custom fields.
     *
     * @return \Assely\Field\Field[]
     */
    public function fields() {
        return [
            Field::selective('favourite_movies', [
                'items' => $movies->all()
            ])
        ];
    }
}
```

<a name="configuring-how-metadata-is-stored"></a>
## [Configuring how metadata is stored](#configuring-how-metadata-is-stored)

Singularities can store it's [metadata](https://codex.wordpress.org/Glossary#Meta) inside database in various way. This configuration determines how you will be accessing these values across your application.

<a name="preserve-as-single-record"></a>
### [Preserve as single record](#preserve-as-single-record)

All metadata are stored in single record and are accessible under one, same key.

```php
public function arguments() {
    return [
        'preserve' => 'single'
    ];
}
```

For example, you have `App\Metaboxes\MovieDetails` with `movie-details` slug and `Field::datepicker('release_date')` field.

```php
use Field;

public function fields() {
    return [
        Field::datepicker('release_date'),
        Field::text('director')
    ];
}
```

The metadata for this singularity will be preserved something like this:

| meta_key | meta_value |
|---|---|
| `movie-details` | `['release_date' => '2016-06-06T00:00:00+00:00', 'director' => 'Gore Verbinski']` |

<a name="preserve-as-multiple-records"></a>
### [Preserve as multiple records](#preserve-as-multiple-records)

Switching to `multiple` causes that metadata are stored in multiple records, where the key will be slug of top level custom field.

```php
public function arguments() {
    return [
        'preserve' => 'multiple'
    ];
}
```

In this case [example form above](#preserve-as-multiple-records) will be stored diffrently.

| meta_key | meta_value |
|---|---|
| `realase_date` | `2016-06-06T00:00:00+00:00` |
| `director` | `Gore Verbinski` |
