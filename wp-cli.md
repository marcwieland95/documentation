- [Introduction](#introduction)
- [Usage](#usage)
- [Commands](#commands)
    + [`wp assely:make`](#make-command)
    + [`wp assely:clear`](#clear-command)
    + [`wp assely:info`](#info-command)
    + [`wp assely:salts`](#salts-command)

<a name="introduction"></a>
## [Introduction](#introduction)

The Assely framework provides a bunch of ready to use WP-CLI commands, which help you develop and debug your application.

<a name="usage"></a>
## [Usage](#usage)

Before running the commands below make sure to navigate into the WordPress root directory (like when using the WP-CLI), otherwise they won't be available.

[alert type="warning"]You should execute commands within the virtual machine, while using Vagrant.[/alert]

For Trellis, you have to login into Vagrant with `vagrant ssh` command and navigate to the `/srv/www/<project-name>/current` directory.

<a name="commands"></a>
## [Commands](#commands)

The Assely framework provides bunch of ready to use WP-CLI commands, which help you develop and debug your application.

[alert type="info"]You can create own custom commands dedicated for your application. Visit [Command](/docs/command) documentation.[/alert]

<a name="make-command"></a>
### [`wp assely:make`](#make-command)

Helps scaffold Assely application singularities.

<hr>

#### Creating controllers

Scaffolds application controller to `app\Http\Controllers` directory.

```bash
wp assely:make controller <classname>
```

<hr>

#### Creating posttypes

Scaffolds application posttype to `app\Posttypes` directory.

```bash
wp assely:make posttype <classname>
```

###### Options

###### `--slug` – Specify posttype slug.

<hr>

#### Creating taxonomies

Scaffolds application taxonomy to `app\Taxonomies` directory.

```bash
wp assely:make taxonomy <classname>
```

###### Options

###### `--slug` – Specify taxonomy slug.
###### `--belongsto` – Specify where taxonomy belongs to.

<hr>

#### Creating metaboxes

Scaffolds application metabox to `app\Metaboxes` directory.

```bash
wp assely:make metabox <classname>
```

###### Options

###### `--slug` – Specify metabox slug.
###### `--belongsto` – Specify where metabox belongs to.

<hr>

#### Creating commands

Scaffolds application command to `app\Console\Commands` directory.

```bash
wp assely:make command <classname>
```

###### Options

###### `--command` – Name of the command.
###### `--method` – Name of the command method.


<a name="clear-command"></a>
### [`wp assely:clear`](#clear-command)

Helps clear Assely application various caches.

<hr>

#### Clearing views cache

Removes all views cache stored in `storage/framework/views`.

```bash
wp assely:clear views
```

<hr>

#### Clearing cache transients

Removes all expired cache transients.

```bash
wp assely:clear cache
```

###### Options

###### `--all` – Clear entire cache transients. Important, it removes all expired and not expired transients.

<hr>

#### Clearing rewrite rules

Flushes application rewrite rules.

```bash
wp assely:clear rewrites
```

<a name="info-command"></a>
### [`wp assely:info`](#info-command)

Provides various command which helps debugging your Assely application.

#### Showing registered routes

Displays all registered application routes.

```bash
wp assely:info routes

+-------------+--------+----------------------+---------------------+--------------+-------------------------------------------+
| Condition   | Filter | Action               | Parameters          | Pattern      | Guid                                      |
+-------------+--------+----------------------+---------------------+--------------+-------------------------------------------+
|             |        | HomeController@index | []                  |              | index.php?                                |
| docs        |        | DocsController@index | []                  | docs         | index.php?post_type=docs                  |
| docs/{name} |        | DocsController@show  | {"name":"([^\/]+)"} | docs/([^/]+) | index.php?post_type=docs&name=$matches[1] |
| 404         |        | {}                   | []                  |              |                                           |
+-------------+--------+----------------------+---------------------+--------------+-------------------------------------------+
```

#### Showing registered assets

Displays all registered application assets in `website` area.

```bash
wp assely:info assets

+----------------------+--------+---------------+-------+-----------+-----------+---------+
| Slug                 | Type   | Path          | Area  | Placement | Execution | Version |
+----------------------+--------+---------------+-------+-----------+-----------+---------+
| app-styles           | style  | css/app.css   | theme | screen    | []        |         |
| app-scripts          | script | js/all.js     | theme | footer    | []        |         |
+----------------------+--------+---------------+-------+-----------+-----------+---------+
```

###### Options

###### `--all` – Display all registered assets across all areas.

```bash
wp assely:info assets --all
```

#### Showing registered menus

Displays all registered application menus.

```bash
wp assely:info menus

+---------+-----------------+--------+
| Slug    | Top level items | Active |
+---------+-----------------+--------+
| primary | 3               | true   |
+---------+-----------------+--------+
```

#### Showing registered sidebars

Displays all registered application menus.

```bash
wp assely:info sidebars

+------+---------------+-----------------------------------------+-------------+
| Slug | Title         | Description                             | Has widgets |
+------+---------------+-----------------------------------------+-------------+
| docs | Documentation | Area below documentation sections menu. | false       |
+------+---------------+-----------------------------------------+-------------+
```

<a name="salts-command"></a>
### [`wp assely:salts`](#salts-command)

Provides helper for managing WordPress salts.

#### Generating salts

```bash
wp assely:salts generate
```

This will output environment variables that you can copy and paste to your .env file.

```bash
AUTH_KEY='random-salt-token'
SECURE_AUTH_KEY='random-salt-token'
LOGGED_IN_KEY='random-salt-token'
NONCE_KEY='random-salt-token'
AUTH_SALT='random-salt-token'
SECURE_AUTH_SALT='random-salt-token'
LOGGED_IN_SALT='random-salt-token'
NONCE_SALT='random-salt-token'
```

###### Options

###### `--format` – Allows for outputing variables in diffrent format. Accepted values: `env`, `yaml`, `constant`.

```bash
wp assely:salts generate --format="constant"
```
