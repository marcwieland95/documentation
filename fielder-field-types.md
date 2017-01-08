- [Field types](#field-types)
    + [Checkboxes](#checkboxes)
    + [Code](#code)
    + [Colorpicker](#colorpicker)
    + [Datepicker](#datepicker)
    + [Editor](#editor)
    + [Hidden](#hidden)
    + [Media](#media)
    + [Password](#password)
    + [Radios](#radios)
    + [Readonly](#readonly)
    + [Repeatable](#repeatable)
    + [Text](#text)
    + [Textarea](#textarea)
    + [Timepicker](#timepicker)
    + [Tinymce](#tinymce)
    + [Toggle](#toggle)


<a name="field-types"></a>
## [Field types](#field-types)

Reference list of predefined custom fields to use with Assely framework.

[alert type="info"]Please, read fielder [usage documentation](/docs/fielder-usage) for general instructions.[/alert]

<a name="checkboxes"></a>
### [Checkboxes](#checkboxes)

Adds multiple checkbox inputs.

```php
Field::checkboxes($slug, $arguments = []);
```

##### Setting items

Use `items` argument for defining checkboxes inputs. It accepts an array, where array key is checkbox name (that will be saved to the database) and array value is text displayed along with checkbox.

```php
Field::checkboxes('checkboxes', [
    'items' => [
        'checkbox-1' => 'First checkbox',
        'checkbox-2' => 'Second checkbox'
    ]
]);
```

##### Setting default values

You can set initial checkboxes values with `default` argument. It should contains array, where array key is checkbox slug and value is checkbox status.

```php
Field::checkboxes('checkboxes', [
    'items' => [
        'checkbox-1' => 'First checkbox',
        'checkbox-2' => 'Second checkbox',
    ],
    'default' => [
        'checkbox-1' => true,
        'checkbox-2' => false,
    ],
]);
```

<a name="code"></a>
### [Code](#code)

Adds code editor.

```php
Field::code($slug, $arguments = []);
```

##### Changing editor mode

By default editor is in `html` mode, but you can change that with `mode` argument. Supported modes are `html`, `css`, `javascript` and `markdown`.

```php
Field::code('code', [
    'mode' => 'css'
]);
```

<a name="colorpicker"></a>
### [Colorpicker](#colorpicker)

Adds colorpicker input.

```php
Field::colorpicker($slug, $arguments = []);
```

<a name="datepicker"></a>
### [Datepicker](#datepicker)

Adds datepicker input.

```php
Field::datepicker($slug, $arguments = []);
```

<a name="editor"></a>
### [Editor](#editor)

Adds text editor.

```php
Field::editor($slug, $arguments = []);
```

<a name="hidden"></a>
### [Hidden](#hidden)

Adds hidden input.

```php
Field::hidden($slug, $arguments = []);
```

<a name="media"></a>
### [Media](#media)

Adds media file input. Uses WordPress Media Library.

```php
Field::media($slug, $arguments = []);
```

##### Setting default values

In order to set default image for media field, you have to define all nesseccary properties in `default` argument.

[alert type="info"]Be dynamic. Use WordPress functions like `wp_get_attachment_url` to find attachments url.[/alert]

```php
Field::media('media', [
    'title' => ['Media field'],
    'default' => [
        'id' => 10,
        'type' => 'image',
        'url' => wp_get_attachment_url(10),
    ],
])
```

<a name="password"></a>
### [Password](#password)

Adds password input.

[alert type="danger"]The password field **do not** hash or encrypt it's value. It is just simple input with `type="password"` attribute. You must take care of it by yourself in sanitization callback.[/alert]

```php
Field::password($slug, $arguments = []);
```

<a name="radios"></a>
### [Radios](#radios)

Adds multiple radio inputs.

```php
Field::radios($slug, $arguments = []);
```

##### Setting items

Use `items` argument for defining radios. It accepts an `key => value` array.

```php
Field::radios('radios', [
    'items' => [
        'radio-1' => 'First radio',
        'radio-2' => 'Second radio'
    ]
]);
```

##### Setting default values

You can set initial value with `default` argument. It should contains key of choosen radio.

```php
Field::radios('radios', [
    'items' => [
        'radio-1' => 'First radio',
        'radio-2' => 'Second radio',
    ],
    'default' => 'radio-2',
]);
```

<a name="readonly"></a>
### [Readonly](#readonly)

Adds readonly field that don't allows for changing it's value.

```php
Field::readonly($slug, $arguments = []);
```

<a name="repeatable"></a>
### [Repeatable](#repeatable)

Adds repeatable field that allows for creating infinite repeated group of fields.

```php
Field::repeatable($slug, $arguments = []);
```

##### Adding children fields to repeatable field

Simply call `children` method with array of fields as argument.

```php
Field::repeatable('repeat')->children([
    Field::text('text'),
    Field::colorpicker('colorpicker'),
]);
```

Of course, repeatable fields **can be** infinitely nested. Yey!

```php
Field::repeatable('repeat')->children([
    Field::repeatable('infinitely')->children([
        Field::text('text'),
    ]);
]);
```

<a name="text"></a>
### [Text](#text)

Adds simple text input. Perfect to store short strings.

[attachment slug="fielder-text-field"]

```php
Field::text($slug, $arguments = [])
```

##### Example

```php
Field::text('text', [
    'default' => 'Why is the rum gone?',
    'description' => 'Asking the important question here.'
])
```

##### Template usage

```html
<h2>{{ $post->meta('text') }}</h2>
```

<a name="textarea"></a>
### [Textarea](#textarea)

Adds textarea input.

```php
Field::textarea($slug, $arguments = []);
```

<a name="timepicker"></a>
### [Timepicker](#timepicker)

Adds timepicker input.

```php
Field::timepicker($slug, $arguments = []);
```

<a name="tinymce"></a>
### [Tinymce](#tinymce)

Adds WorPress Tinymce editor.

[alert type="warning"]Because of WordPress limitations, the Tinymce field don't work inside repeatable fields. However, you may use the [Editor field](#editor) as substitute.[/alert]

```php
Field::tinymce($slug, $arguments = []);
```

<a name="toggle"></a>
### [Toggle](#toggle)

Adds toggle switch input.

```php
Field::toggle($slug, $arguments = []);
```

