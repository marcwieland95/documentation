- [Introduction](#introduction)
- [Columns on the list of Users](#columns-on-the-list-of-users)
- [Using Users Repository](#using-user-repository)
    + [Quering Users](#quering-users)
    + [Inserting Users](#inserting-users)
    + [Updating Users](#updating-users)
    + [Deleting Users](#deleting-users)
    + [Working with User roles](#working-with-user-roles)
    + [Rendering Users in templates](#rendering-user-in-templates)


<a name="introduction"></a>
## [Introduction](#introduction)

The Assely framework comes with `App\Users` repository class that allows for managing WordPress users.

This singularity, unlike other, is registered by default inside `App\Providers\SingularityServiceProvider` class.

<a name="columns-on-the-list-of-users"></a>
## [Columns on the list of Users](#columns-on-the-list-of-users)

The `columns` method inside `app\Users.php` file allows to define custom columns which are displayed on the list of users.

Users accepts one kind of column: user.

```php
public function columns()
{
    return [
        Column::user('App\Profiles\MovieCritic', 'score'),
    ];
}
```

[alert type="info"]More accurate description about `Column` facade you can find in [column documentation](/docs/column).[/alert]

<a name="using-users-repository"></a>
## [Using Users Repository](#using-users-repository)

`User` facade provides access to the users. However, you can also inject the `App\Users` repository class.

```php
use App\Users;

public function index(Users $users) {
    $user = $users->find(1);
}
```

<a name="quering-user"></a>
### [Quering User](#quering-user)

In this section you will learn how you can query users of your application.

#### Fetching multiple Users

The `all` method will retrive all of the users. This method returns an instance of `Illuminate\Support\Collection`, so you can easily modify results with diffrent [helper methods](https://laravel.com/docs/5.2/collections#available-methods).

```php
$users = User::all();
```

#### Retrieving single User

You can also immediately search for single user. Pass user id as the `find` method argument.

```php
$user = User::find(1);
```

##### Finding Users by a given fields

The `findBy` method allows for finding users by they profile field. You can use `id`, `slug`, `login` or even `email`.

```php
User::findBy('email', 'user@example.com')
```

##### Not Found Query Exception

Sometimes you want to throw an exception when user was not found. The `findOrFail` method returns found user, but if not, the `Assely\Database\QueryException` will be thrown.

```php
User::findOrFail(1);
```

#### Custom Query

Needs something more complex? You can use `query` method with array of [ parameters](https://codex.wordpress.org/Class_Reference/WP_User_Query#Parameters) as argument.

```php
User::query([
    'date_query' => [
        [
            'after' => '12 hours ago',
            'inclusive' => true
        ]
    ]
]);
```

<a name="inserting-users"></a>
### [Inserting Users](#inserting-users)

Use `create` method with array of properties and values for inserting new user to the database.

```php
User::create([
    'email' => 'john.smith@example.com',
]);
```

##### Inserting Query Exception

The `createdOrFail` method will insert new user, but when error occurs, the `Assely\Database\QueryException` will be thrown.

```php
User::createOrFail([
    'email' => 'john.smith@example.com',
]);
```

<a name="updating-users"></a>
### [Updating Users](#updating-users)

To update a user, you need to set new property values on retrived `Assely\Adapter\User` instance, and then call `save` method.

```php
$user = User::find(1);

$user->name = 'John Smith';

$user->save();
```

<a name="deleting-users"></a>
### [Deleting User](#deleting-users)

Call `destroy` method on retrieved `Assely\Adapter\User` instance to remove user from database.

```php
$user = User::find(1);

$user->destroy();
```

<a name="working-with-user-roles"></a>
### [Working with User roles](#working-with-user-roles)

WordPress users can have diffrent roles and capabilities. You can verify what users can and cannot do.

##### Getting User roles

```php
$user_roles = User::find(1)->roles;
```

##### Verifing User roles

With `hasRole` method you can check if user has specifed role.

```php
$user = User::find(1);

if ($user->hasRole('Administrator')) {
    // Yes, he is an administrator.
}
```

##### Verifing User capabilities

You can verify what users can and cannot do with `can` method. Reference to the [Codex](https://codex.wordpress.org/Roles_and_Capabilities#Capabilities) for list of all available compabilities.

```php
$user = User::find(1);

if ($user->can('edit_posts')) {
    // Yes, he can edit posts.
}
```


<a name="rendering-user-in-templates"></a>
### [Rendering Users in templates](#rendering-user-in-templates)

You have access to bunch of diffrent properties. List of all you can find in [adapters documentation](/docs/adapters#user).

```html
<div class="user">
    <h2>{{ $user->name }}</h2>
    <span>{{ $user->email }}</span>
</div>
```

#### Accessing Users metadata

Your users may hold various metadata. The `meta` method helps you retrieve these informations. You only need to pass metadata key under which they are stored.

```html
{{ $user->meta('score') }}
```
