- [Introduction](#introduction)
- [Creating Command](#creating-command)
    + [Dependences Injection](#dependences-injection)
    + [Accessing Application and Config Instance](#accessing-application-and-config-instance)
- [Registering Command](#registering-command)
- [Basics of Commands](#basics-of-commands)
    + [Writing to the console](#writing-to-the-console)
    + [Prompting for input](#prompting-for-input)
    + [Getting Command Arguments](#getting-command-arguments)
    + [Getting Command Options](#getting-command-options)
    + [Calling other Commands](#calling-other-commands)


<a name="introduction"></a>
## [Introduction](#introduction)

The Command component allows for convenient way to create custom commands for your application. It's wrapped around powerfull [WP-CLI](http://wp-cli.org), so you can bring into play it's all features.

[alert type="info"]Framework provides a bunch of ready to use commands. To view a list of all available commands, visit the [WP-CLI documentation page](/docs/wp-cli).[/alert]

<a name="creating-command"></a>
## [Creating Command](#creating-command)

To create a new command, you can use the `wp assely:make command` command, which will generate a command class for you.

It requires two mandatory options:

- `--signature` - Command signature under which custom command will registered.
- `--method` - Method name inside class which handles command logic.

```
wp assely:make command Movies --signature=app:movies --method=fetch
```

Command above will scaffold `Movies` class inside `app\Console\Commands` directory. It also includes basic PHPDoc annotation to help you started.

##### Command signature

The `signature` property defines command name under which your custom command will be callable in terminal.

[alert type="warning"]It's better to namespace your commands with some sort of prefix, eg. `app:movies`.[/alert]

```php
<?php

namespace App\Console\Commands;

use Assely\Foundation\Console\Command;

class Movies extends Command
{
    /**
     * Command singnature.
     *
     * @var string
     */
    public $signature = 'app:movies';

    /**
     * Fetching movies details from API.
     *
     * ## OPTIONS
     *
     * <ratings>
     * : Fetching ratings.
     *
     * [--above=<number>]
     * : Movies with rating above specifed number.
     *
     * ## EXAMPLE
     *
     *     wp app:movies fetch ratings --above=5
     *
     */
    public function fetch()
    {
        // command logic
    }
}
```

<a name="dependences-injection"></a>
### [Dependences Injection](#dependences-injection)

You are able to type-hint command dependences in the class constructor or method. They will be resolved from the container and injected for you.

```php
<?php

namespace App\Console\Commands;

use GuzzleHttp\Client;
use Illuminate\Filesystem\Filesystem;
use Assely\Foundation\Console\Command;

class Movies extends Command
{
    /**
     * Command singnature.
     *
     * @var string
     */
    public $signature = 'app:movies';

    /**
     * Construct command.
     *
     * @param \Illuminate\Filesystem\Filesystem $filesystem
     */
    public function __construct(Filesystem $filesystem)
    {
        $this->filesystem = $filesystem;
    }

    /**
     * Fetching movies details from API.
     *
     * @param \GuzzleHttp\Client $client
     */
    public function fetch(Client $client)
    {
        $client->request('GET', 'https://api.example.com');
    }
}
```

<a name="accessing-application-and-config-instance"></a>
### [Accessing Application and Config Instance](#accessing-application-and-config-instance)

Inside your command class you have directly access to the application and config instance. Simply reference they properties.

```php
<?php

namespace App\Console\Commands;

use Assely\Foundation\Console\Command;

class Movies extends Command
{
    /**
     * Command singnature.
     *
     * @var string
     */
    public $signature = 'app:movies';

    /**
     * Fetching movies details from IMDb.
     */
    public function fetch()
    {
        // Application instance
        var_dump($this->app);

        // Application configs
        var_dump($this->config);
    }
}
```

<a name="registering-command"></a>
## [Registering Command](#registering-command)

Once you have command created, you need to register it within application. Go to the application `ConsoleServiceProvider` class which you can find inside `app\Providers` directory.

In order to register your command, add command classname to the `commands` property array like bellow.

```php
protected $commands = [
    'App\Console\Commands\Movies',
];
```

<a name="basics-of-commands"></a>
## [Basics of commands](#basics-of-commands)

<a name="writing-to-the-console"></a>
### [Writing to the console](#writing-to-the-console)

Allows to display an information messages for the user.

#### Outputting lines

To output line to the console, simply use `line` method. This will display your message as standard text.

```php
$this->line('Display normal text line.');
```

However, you may want to colorize output for better readability. Call `colorize` method with message as first argument and color code as second. [WP-CLI colorize documentation](http://wp-cli.org/docs/internal-api/wp-cli-colorize/#notes) contains full color codes reference.

```php
$this->line($this->colorize('Warning: ', '%b') . "Be careful.");
```

##### Standard messages

Of course, there are few ready to use methods for standard messaging.

```php
$this->success('Good. Everything went well.');
```

```php
$this->error('Ops. We have a problem.');
```

#### Outputting table

The `table` method is handy for outputting nicely formated tabular data.

```php
$headers = ['Title', 'Release date'];

$dataset = [
    ['Pirates of the Caribbean', '2003'],
];

$this->table($headers, $dataset);
```

<a name="prompting-for-input"></a>
### [Prompting for input](#prompting-for-input)

Your commands can ask user for input, which you can use durning command execution. Use `ask` method to display and return the user's input.

```php
$favourite = $this->ask('Your favourite movie');
```

You can specify default value to be return if user do not provide any input.

```php
$this->ask('Your favourite movie', $default);
```

#### Asking for confirmation

Sometimes you only want to ask the user for simple "yes" or "no" confirmation. The `confirm` method will continue command execution on "yes" or abort if user chooses "no".

```php
$this->confirm('Are you sure you want to archive this movie?');
```

#### Asking for choise

You can also force the user to make a choice. Use `choice` method where second argument is an array of options from which the user have to choose.

```php
$favourite = $this->choice('Choose you favourite genre', [
    'comedy' => 'Comedy',
    'fantasy' => 'Fantasy',
]);
```

<a name="getting-command-arguments"></a>
### [Getting command arguments](#getting-command-arguments)

Arguments is a simple array, where successive arguments are ordered on zero-based indexes.

For example, in `wp app:movies fetch ratings` command, it's arguments will be an `['ratings']` array.

```php
// Returns `['ratings']` array.
$this->getArguments();
```

Retrive specifed arguments by it's position indexes:

```php
// Returns `ratings` string.
$this->getArgument(0);
```

<a name="getting-command-options"></a>
### [Getting command options](#getting-command-options)

You can easily access command options with `getOption` method and they names. You may also provide the default value to be returned if option was not set.

```php
$this->getOption($name, $default);
```

<a name="calling-other-commands"></a>
### [Calling other commands](#calling-other-commands)

The `call` method allows for programmatically execute other registered commands.

```php
$this->call('theme get');
```

Options can be passed in the second argument as array of option key and value.

```php
$this->call('theme get', ['field' => 'name']);
```
