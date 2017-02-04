- [Configuration](#configuration)
    + [Informations](#informations)
	+ [Config files](#config-files)
    + [Theme Support Options](#theme-support-options)
    + [Screenshot](#screenshot)
- [Additional Informations](#additional-informations)
    + [Fielder](#fielder)


<a name="configuration"></a>
## [Configuration](#configuration)

<a name="informations"></a>
### [Informations](#informations)

As same as standard theme, we need to fill theme informations inside `style.css` file. All necessary guides you will find in [Codex](https://codex.wordpress.org/Theme_Development#Theme_Stylesheet).

[alert type="danger"]`styles.css` file is responsible **only** for providing theme informations that are displayed in `Apperance > Themes`. Do not write in here any css declarations.[/alert]

<a name="config-files"></a>
### [Config files](#config-files)

All application configs are located inside `config` directory. Every option has short description, scan this files in order to know what each do.

<a name="theme-support-options"></a>
### [Theme Support Options](#theme-support-options)

[alert type="info"]Take a look at [Codex](https://developer.wordpress.org/reference/functions/add_theme_support/#more-information) for all avaiable support options.[/alert]

We enabled most common theme supports for you in `app\Providers\AppServiceProvider` class and `addSupport` method. However, feel free to add your own entries there.

To enable additonal option, simple add new position to the array where key is option slug and value is option argument.

```php
protected function addSupport()
{
    Support::add([
        'title-tag' => true,
    ]);
}
```

[alert type="info"]Use `true` if option do not have any specific argument value.[/alert]

<a name="screenshot"></a>
### [Screenshot](#screenshot)

Along with styles.css there is also standard `screenshot.png` image. Change it if you want a custom theme thumbnail.

<a name="additional-informations"></a>
## [Additional Informations](#additional-informations)

<a name="fielder"></a>
### [Fielder](#fielder)

All Assely framework custom fields are extracted into own separated plugin called [Fielder](https://wordpress.org/plugins/fielder/). In order to be able to add custom fields (highly likely, yes) you need to pull in this plugin too. Read more in [fielder docs](/docs/fielder/#install).
