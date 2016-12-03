- [Introduction](#introduction)
	+ [Installing Bedrock](#installing-bedrock)
    + [Scaffolding Assely application](#scaffolding-assely-application)
    + [Requiring packages](#requiring-packages)
    + [Autoloading application](#autoloading-application)
    + [Installing Composer dependences](#installing-composer-dependences)


<a name="introduction"></a>
## [Introduction](#introduction)

The Assely framework perfectly integrates with [Bedrock](https://roots.io/bedrock/). It's recommended stack in order to take full advantage of the framework, like environment files and WP-CLI commands.

<a name="installing-bedrock"></a>
### [Installing Bedrock](#installing-bedrock)

Basically follow up the [Bedrock documentation](https://roots.io/bedrock/docs/installing-bedrock/). It is simple and well-explanatory.

<a name="scaffolding-assely-application"></a>
### [Scaffolding Assely application](#scaffolding-assely-application)

[alert type="info"]This instruction use our [installer](https://github.com/assely/installer). However, detailed informations about other ways of creating new Assely applications you will find in [general installation documentation](#installing).[/alert]

Now we can scaffold our application. Bedrock holds all themes in `web/app/themes` folder. Go to that directory and run `assely new <project-name>` command to craft fresh project.

```bash
cd web/app/themes

assely new project-name
```

<a name="requiring-package"></a>
### [Requiring package](#requiring-package)

[alert type="danger"] ***Do not install or require*** composer dependences inside your application `composer.json`. You should use Bedrock's `composer.json` itself.[/alert]

Now, we will be working with Bedrock's `composer.json` file. Get back to the bedrock root folder and require the `assely/framework` package:

```bash
composer require assely/framework
```

.. or manualy add package entry to the require section in `composer.json` file.

```json
"require": {
    "assely/framework": "^0.1.0",
}
```

<a name="autoloading-application"></a>
### [Autoloading application](#autoloading-application)

In order to properly load project files, we must tell composer where he can find it. Add PSR-4 autoload rules for your application in the Bedrock's `composer.json` file. Insert `autoload` section with namespace and path of application directory.

```json
"autoload": {
    "psr-4": {
        "App\\": "web/app/themes/<project-name>/app"
    }
}
```

<a name="installing-composer-dependences"></a>
### [Installing Composer dependences](#installing-composer-dependences)

Finally, last step. While still being in the Bedrock's root folder run `composer install` to resolve project dependences and generate autoloading files.

[alert type="info"]The Assely framework packages are installed as plugins in `web/app/plugins` directory.[/alert]

You are ready to start. Activate newly created theme and visit your website.
