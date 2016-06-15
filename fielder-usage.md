- [Usage](#usage)
    + [Field title](#field-title)
    + [Field description](#field-description)
    + [Field default value](#field-default-value)
    + [Field items](#field-items)
    + [Defining children fields](#defining-children-fields)
    + [Displaying fields in grid](#displaying-fields-in-grid)
- [Validating fields](#validating-fields)
    + [Available validators](#available-validators)
- [Sanitizing fields values](#sanitizing-fields-values)


<a name="usage"></a>
## [Usage](#usage)

It's fair simple. Call `Field` facade with field type as method name. Pass desired field slug as first argument and accepted options in second argument.

```php
Field::text('text', [
    'title' => ['Text field']
]);
```

I've told you it will be simple :)

<a name="field-title"></a>
### [Field title](#field-title)

By default, title is generated based on slug. However, you can specify it with `title` argument.

[alert type="info"]Title argument value must be an array, where first element is singular variety and second is plural variety.[/alert]

```php
Field::text('text', [
    'title' => ['Inspiring title']
]);
```

<a name="field-description"></a>
### [Field description](#field-description)

Description is displayed above input. It's usefull for providing additional explanations about field for the users.

```php
Field::text('text', [
    'description' => 'This is a descriptive description about field.'
]);
```

<a name="field-default-value"></a>
### [Field default value](#field-default-value)

You can define initial field values with `default` argument.

```php
Field::text('text', [
    'default' => "Why is the Rum Gone?"
]);
```

<a name="defining-children-fields"></a>
### [Defining children fields](#defining-children-fields)

Some fields like `checkboxes` or `radios` have multiple entries and you can specify it with `items` argument.

[alert type="info"]The array structure of the `items` argument may be diffrent for each field type. Follow directly [field types documentation](/docs/fielder-field-types) for more informations.[/alert]

```php
Field::checkboxes('checkboxes', [
    'items' => [
        'checkbox-1' => 'First checkbox',
        'checkbox-2' => 'Second checkbox'
    ]
]);
```

<a name="defining-children-fields"></a>
### [Defining children fields](#defining-children-fields)

If field handles children fields, they can be defined with `children` method and array of fields as argument.

```php
Field::repeatable('repeatable')->children([
    Field::text('text'),
]);
```

<a name="displaying-fields-in-grid"></a>
### [Displaying fields in grid](#displaying-fields-in-grid)

Normally, each field takes the whole row. However you can change this behaviour with `column` argument.

For example, field with column set to `1-2` will take half of the row.

```php
Field::text('text' [
    'column' => '1-2'
]);
```

Full grid units reference you can find in [Pure documentation](http://purecss.io/grids/#grids-units-sizes)

<a name="validation"></a>
## [Validating fields](#validation)

Fields validation is done by [Parsley](http://parsleyjs.org) and you can use it's various [validators](#available-validators). Call `validate` method with array of validators as argument.

```php
Field::text('quote')->validate(['required']);
```

Validators with arguments needs to be in specific schema, where name and argument are separated with colon: `<name>:<argument>`.

```php
Field::text('quote')->validate(['max:10']);
```

<a name="available-validators"></a>
### [Available validators](#available-validators)

| Validator | Description |
|---|---|
| `required` | Validates that a required field has been filled with a non blank value. |
| `type:email` | Validates that a value is a valid email address. |
| `type:url` | Validates that a value is a valid url. |
| `type:number` | Validates that a value is a valid number. |
| `type:integer` | Validates that a value is a valid integer. |
| `type:digits` | Validates that a value is only digits. |
| `type:alphanum` | Validates that a value is a valid alphanumeric string. |
| `min:6` | Validates that a given number is greater than or equal to some minimum number. |
| `max:6` | Validates that a given number is less than or equal to some maximum number. |
| `rage:[6, 12]` | Validates that a given number is between some minimum and maximum number. |
| `mincheck:3` | Validates that a certain minimum number of checkboxes in a group are checked. |
| `maxcheck:3` | Validates that a certain maximum number of checkboxes in a group are checked. |
| `check:[1, 3]` |  Validates that the number of checked checkboxes in a group is within a certain range. |
| `pattern:\d+` | Validates that a value matches a specific regular expression (regex). |

<a name="sanitization"></a>
## [Sanitizing fields values](#sanitization)

Fields values can be sanitized before saving to the database. Use `sanitize` method with Closure as argument. Callback needs to return field value, of course after desirable modifications.

```php
Field::text('quote')->sanitize(function ($value) {
    return ucfirst($value);
});
```

You can also use WordPress buildin [sanitization functions](https://codex.wordpress.org/Validating_Sanitizing_and_Escaping_User_Data#Sanitizing:_Cleaning_User_Input)

```php
Field::text('quote')->sanitize('sanitize_text_field');
```

When you call `sanitize` method without any arguments, the `sanitize_text_field` function will be used.

```php
// uses sanitize_text_field()
Field::text('quote')->sanitize();
```
