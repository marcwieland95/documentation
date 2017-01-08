- [Introduction](#introduction)
- [Opening A Form](#opening-a-form)
- [Labels](#labels)
- [Text, Text Area, Password & Hidden Fields](#text)
- [Checkboxes and Radio Buttons](#checkboxes-and-radio-buttons)
- [File Input](#file-input)
- [Number Input](#number)
- [Drop-Down Lists](#drop-down-lists)
- [Buttons](#buttons)
- [Custom Macros](#custom-macros)


<a name="introduction"></a>
## [Introduction](#introduction)

The HTML service is fork of [Laravel HTML 4.2](https://laravel.com/docs/4.2/html) component. It provides various functions for rapidly building application forms.

<a name="opening-a-form"></a>
## [Opening A Form](#opening-a-form)

```html
{!! Form::open(['url' => 'foo/bar']) !!}
    //
{!! Form::close() !!}
```

By default, a `POST` method will be assumed; however, you are free to specify another method:

```html
{!! Form::open(['url' => 'foo/bar', 'method' => 'get']) !!}
```

[alert type="info"]HTML forms only support `POST` and `GET`.[/alert]

If your form is going to accept file uploads, add a `files` option to your array:

```html
{!! Form::open(['url' => 'foo/bar', 'files' => true]) !!}
```

<a name="labels"></a>
## [Labels](#labels)

#### Generating A Label Element

```html
{!! Form::label('email', 'E-Mail Address') !!}
```

#### Specifying Extra HTML Attributes

```html
{!! Form::label('email', 'E-Mail Address', ['class' => 'awesome']) !!}
```

[alert type="info"]After creating a label, any form element you create with a name matching the label name will automatically receive an ID matching the label name as well.[/alert]

<a name="text"></a>
## [Text, Text Area, Password & Hidden Fields](#text)

#### Generating A Text Input

```html
{!! Form::text('username') !!}
```

#### Specifying A Default Value

```html
{!! Form::text('email', 'example@gmail.com') !!}
```

[alert type="info"]The *hidden* and *textarea* methods have the same signature as the *text* method.[/alert]

#### Generating A Password Input

```html
{!! Form::password('password') !!}
```

#### Generating Other Inputs

```html
{!! Form::email($name, $value = null, $attributes = []) !!}
Form::file($name, $attributes = []) !!}
```

<a name="checkboxes-and-radio-buttons"></a>
## [Checkboxes and Radio Buttons](#checkboxes-and-radio-buttons)

#### Generating A Checkbox Or Radio Input

```html
{!! Form::checkbox('name', 'value') !!}
```

```html
{!! Form::radio('name', 'value') !!}
```

#### Generating A Checkbox Or Radio Input That Is Checked

```html
{!! Form::checkbox('name', 'value', true) !!}
```

```html
{!! Form::radio('name', 'value', true) !!}
```

<a name="number"></a>
## [Number](#number)

#### Generating A Number Input

```html
{!! Form::number('name', 'value') !!}
```

<a name="file-input"></a>
## [File Input](#file-input)

#### Generating A File Input

```html
{!! Form::file('image') !!}
```

[alert type="info"]The form must have been opened with the `files` option set to `true`.[/alert]

<a name="drop-down-lists"></a>
## [Drop-Down Lists](#drop-down-lists)

#### Generating A Drop-Down List

```html
{!! Form::select('size', ['L' => 'Large', 'S' => 'Small']) !!}
```

#### Generating A Drop-Down List With Selected Default

```html
{!! Form::select('size', ['L' => 'Large', 'S' => 'Small'], 'S') !!}
```

#### Generating A Grouped List

```html
{!! Form::select('animal', [
    'Cats' => ['leopard' => 'Leopard'],
    'Dogs' => ['spaniel' => 'Spaniel'],
]) !!}
```

#### Generating A Drop-Down List With A Range

```html
{!! Form::selectRange('number', 10, 20) !!}
```

#### Generating A List With Month Names

```html
{!! Form::selectMonth('month') !!}
```

<a name="buttons"></a>
## [Buttons](#buttons)

#### Generating A Submit Button

```html
{!! Form::submit('Click Me!') !!}
```

[alert type="info"]Need to create a button element? Try the *button* method. It has the same signature as *submit*.[/alert]

<a name="custom-macros"></a>
## [Custom Macros](#custom-macros)

#### Registering A Form Macro

It's easy to define your own custom Form class helpers called "macros". Here's how it works. First, simply register the macro with a given name and a Closure:

```php
Form::macro('myField', function() {
    return '<input type="awesome">';
});
```

Now you can call your macro using its name:

#### Calling A Custom Form Macro

```html
{!! Form::myField() !!}
```