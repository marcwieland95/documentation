- [Installation](#installation)
    + [Requiring package](#requiring-package)
    + [Activating plugin](#activating-plugin)
    + [Registering Provider](#registering-provider)
    + [Registering Facade alias](#registering-facade-alias)

<a name="installation"></a>
## [Installation](#installation)

Fielder can be installed as standard WordPress plugin or Composer package. This documentation will guide you through entire process for the both cases.

<a name="requiring-package"></a>
### [Requiring package](#requiring-package)

#### Via Assely Installer

[alert type="warning"]The Assely Installer need to be globaly installed on your system. Visit [installation documentation](/docs/installation) for more info.[/alert]

Change directory to the `wp-content/plugins` and run `assely fetch:fielder`. This command will download lastest release of the Fielder to current directory and resolve its composer dependences for you.

```bash
cd wp-content/plugins

assely fetch:fielder
```

#### Via Composer

[alert type="info"]If you are using Bedrock, add package entry to the Bedrock's main `composer.js` file.[/alert]

You have to require Fielder package. Add `assely/fielder` to your composer.json file or run command:

```bash
composer require assely/fielder
```

<a name="activating-plugin"></a>
### [Activating plugin](#activating-plugin)

Now, we have Fielder plugin pulled in, but it's inactive. Go to the "Plugins" dashboard and click activate. As always, you may also use WP-CLI:

```bash
wp plugin activate assely-fielder
```

<a name="registering-provider"></a>
### [Registering Provider](#registering-provider)

Open `config/app.php` and, within the providers array, add `FielderServiceProvider`.

```php
'providers' => [
    // ...other providers

    Assely\Fielder\FielderServiceProvider::class
]
```

<a name="registering-facade-alias"></a>
### [Registering Facade alias](#registering-facade-alias)

The last thing, register `Field` facade. Inside `config/app.php` in `aliases` array add:

```php
'aliases' => [
    // ...other aliases

    'Field' => 'Assely\Fielder\Support\Facades\Field'
]
```
