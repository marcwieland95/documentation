- [Getting Authenticated User](#getting-authenticated-user)
- [Checking If The Current User Is Authenticated](#checking-if-the-current-user-is-authenticated)
- [Authenticating Users](#authenticating-users)
    + [Authenticate with User instance](#authenticate-with-user-instance)
    + [Authenticate with credentials](#authenticate-with-credentials)
    + [Logout Authenticated User](#logout-authenticated-user)


<a name="getting-authenticated-user"></a>
## [Getting Authenticated User](#getting-authenticated-user)

Use `user` method to receive currently logged in user instance.

```php
$user = Auth::user();
```

<a name="checking-if-the-current-user-is-authenticated"></a>
## [Checking If The Current User Is Authenticated](#checking-if-the-current-user-is-authenticated)

You can quickly check authentication status with `check` method.

```php
if (Auth::check()) {
    // ...have authenticated user
}
```

<a name="authenticating-users"></a>
## [Authenticating Users](#authenticating-users)

<a name="authenticate-with-user-instance"></a>
### [Authenticate with User instance](#authenticate-with-user-instance)

You may want to login exsiting user with his `Assely\Adapter\User` instance.

```php
$user = User::find(1);

// Login user
Auth::login($user);

// Login and remember user
Auth::login($user, true);
```

<a name="authenticate-with-credentials"></a>
### [Authenticate with credentials](#authenticate-with-credentials)

You can attempt to login user with credentials. `attempt` method accepts array of user login and password values. If attempt was successful `true` is returned, if not - `false`.

```php
// Attemp login
Auth::attempt([
    'user_login' => 'admin',
    'user_password' => 'plainpassword',
]);

// Attemp login and remember
Auth::attempt([
    'user_login' => 'admin',
    'user_password' => 'plainpassword',
], true);
```

<a name="logout-authenticated-user"></a>
### [Logout Authenticated User](#logout-authenticated-user)

To logout current user simply call `logout` method.

```php
Auth::logout();
```
