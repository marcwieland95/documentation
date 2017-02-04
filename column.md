- [Introduction](#introduction)
- [Registering Columns](#registering-columns)
- [Column types](#column-types)
    + [Metabox Column](#metabox-column)
    + [Taxonomy Column](#taxonomy-column)
    + [Term Column](#term-column)
    + [Profile Column](#profile-column)
- [Accessing fields for Columns](#accessing-fields-for-columns)
    + [Pulling fields with "dot" notation](#pulling-out-fields-with-dot-notation)


<a name="introduction"></a>
## [Introduction](#introduction)

Custom Columns give you ability to show your entries meta values immediately on the list screens of singularities.

<a name="registering-columns"></a>
## [Registering Columns](#registering-columns)

Each singularity that supports custom columns have `columns` method. This method needs to return an array of `Column` instances.

```php
use Column;

public function columns() {
    return [
        Column::taxonomy('App\Taxonomies\Genres'),
    ];
}
```

<a name="column-types"></a>
## [Column types](#column-types)

List of singularites and they supported custom column types:

| Singularity type | Supported column types |
|---|---|
| Posttype | `metabox`, `taxonomy` |
| Taxonomy | `term` |
| User | `profile` |

<a name="metabox-column"></a>
### [Metabox Column](#metabox-column)

Metabox column allows for presenting metadata values of your posts. It requires two arguments. First is desired metabox classname, second is a [field depth path](#pulling-out-fields-with-dot-notation) in "dot" notation.

```php
Column::metabox($metabox, $field);
```
Example:

```php
Column::metabox('App\Metaboxes\MovieDetails', 'release_date');
```

<a name="taxonomy-column"></a>
### [Taxonomy Column](#taxonomy-column)

Taxonomy column allows for presenting choosen terms of your posts taxonomies.

```php
Column::taxonomy($taxonomy);
```

Example:

```php
Column::taxonomy('App\Taxonomies\Genres');
```

<a name="term-column"></a>
### [Term Column](#term-column)

Term column allows for displaying metadata values of your taxonomy terms. It requires two arguments. First is desired taxonomy classname, second is a [field depth path](#pulling-out-fields-with-dot-notation) in "dot" notation.

```php
Column::term($term, $field);
```
Example:

```php
Column::term('App\Taxonomies\Genres', 'color');
```

<a name="profile-column"></a>
### [Profile Column](#profile-column)

Profile column allows for presenting metadata values of your users profiles. It requires two arguments. First is desired taxonomy classname, second is a [field depth path](#pulling-out-fields-with-dot-notation) in "dot" notation.

```php
Column::profile($profile, $field);
```
Example:

```php
Column::profile('App\Profile\Reviewer', 'rank');
```

<a name="accessing-fields-for-columns"></a>
## [Accessing fields for Columns](#accessing-fields-for-columns)

<a name="pulling-out-fields-with-dot-notation"></a>
### [Pulling fields with "dot" notation](#pulling-out-fields-with-dot-notation)

Some Column can display fields previews, in this situation, the second argument will accept depth path in "dot notation".

For example, you organized fields within [Fielder](/docs/introduction) `tabs` and want to display the `text` field which is placed inside one of `tab`.

```php
use Field;

Field::tabs('tabs')->children([
    Field::tab('tab')->children([
        Field::text('text'),
    ]),
]),
```

The depth path to this field will look like this `tabs.tab.text`. Easy, right?

```php
Column::metabox('App\Metaboxes\CustomMetabox', 'tabs.tab.text')
```
