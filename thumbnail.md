- [Introduction](#introduction)
- [Basics of thumbnails](#basics-of-thumbnails)
    + [Creating thumbnail](#creating-thumbnail)
    + [Getting thumbnail](#getting-thumbnail)
    + [Removing thumbnail](#removing-thumbnail)


<a name="introduction"></a>
## [Introduction](#introduction)

The [Post Thumbnails](https://codex.wordpress.org/Post_Thumbnails) are an images that are chosen as the representative for post.

Registering own thumbnails allows for creating additional images sizes, beyond those shipped with WordPress.

<a name="basics-of-thumbnails"></a>
## [Basics of thumbnails](#basics-of-thumbnails)

All application thumbnails should be defined inside `app\Support\thumbnails.php` file. This file is included on application bootstrap by the `App\Providers\AppServiceProvider` class.

<a name="creating-thumbnail"></a>
### [Creating thumbnail](#creating-thumbnail)

Call `create` method on `Thumbnail` facade and pass desired image sizes in arguments.

```php
Thumbnail::create('hero', [
    'size' => [800, 600]
]);
```

#### Arguments

Create method accepts options as second argument.

```php
Thumbnail::create('hero', [
    'title' => ['Hero image'],
    'crop' => false,
]);
```

##### Image Cropping

The `crop` argument changes how the images are resized. More about diffrent cropping options you can read in [Codex](https://developer.wordpress.org/reference/functions/add_image_size/#crop-mode).

##### List of available options:

| Option name | Default value | Description |
|---------|---------|---------|
| title | `[]` | Array of titles, where first element is singular variant, second a plural |
| crop | `false` | Cropping behavior for the image. |
| size | `[800, 600]` | Array of image dimensions (width × height). |

<a name="getting-thumbnail"></a>
### [Getting thumbnail](#getting-thumbnail)

You can get thumbnail instance with `get` method.

```php
Thumbnail::get('hero');
```

<a name="removing-thumbnail"></a>
### [Removing thumbnail](#removing-thumbnail)

Call `remove` method on the desired thumbnail instance to remove previously created thumbnail size.

```php
Thumbnail::get('hero')->remove();
```
