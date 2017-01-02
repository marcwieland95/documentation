- [Introduction](#introduction)
- [Creating Hooks](#creating-hooks)
    + [Action Hooks](#action-hooks)
    + [Filter Hooks](#filter-hooks)
    + [Callback Functions](#callback-functions)
    + [Piority and number of arguments](#piority-and-number-of-arguments)
- [Basics of Hooking](#basics-of-hooking)
    + [Dispaching Hooks](#dispaching-hooks)
    + [Performing Hooks](#performing-hooks)
    + [Removing Hooks](#removing-hooks)


<a name="introduction"></a>
## [Introduction](#introduction)

Hooks, as the [definition](http://codex.wordpress.org/Glossary#Hook) says, are events that call the action or filter functions, previously hooked to that events. In other words, [Hook API](http://codex.wordpress.org/Plugin_API) allows you to trigger yours application logic at specific times.

<a name="creating-hooks"></a>
## [Creating Hooks](#creating-hooks)

Injecting the `Assely\Hook\HookFactory` provides access to the hook service. However, you can also use `Hook` facade.

```php
use Assely\Hook\HookFactory;

public function __construct(HookFactory $hook) {
    $this->hook = $hook;
}

public function register() {
    $this->hook->action('init', [$this, 'methodName'])->dispatch();
}
```

<a name="action-hooks"></a>
### [Action Hooks](#action-hooks)

Call `action` method for creating an action. Codex has a comprehensive [actions reference list](http://codex.wordpress.org/Plugin_API/Action_Reference).

```php
Hook::action($name, $callback);
```

<a name="filter-hooks"></a>
### [Filter Hooks](#filter-hooks)

Call `filter` method for creating an action. The codex has a comprehensive [filters reference list](http://codex.wordpress.org/Plugin_API/Filter_Reference).

```php
Hook::filter($name, $callback);
```

<a name="callback-functions"></a>
### [Callback Functions](#callback-functions)

The Hook callback function can be defined in various ways. There is no recommendations, use the best in a given situation.

#### Method name

To reference a class method pass an array where the first element is a class instance and the second is a method name.

```php
use Hook;

public function __constuct() {
    Hook::action($name, [$this, 'methodName']);
}

public function methodName {
    //
}
```

#### Closure

Sometimes you may need the easiest solution. Passing a `Closure` simply does the job.

```php
Hook::action($name, function() {
    //
});
```

#### Function name

Another option is also a standard function.

```php
function functionName() {
    //
}

Hook::action($name, 'functionName');
```

<a name="piority-and-number-of-arguments"></a>
### [Piority and number of arguments](#piority-and-number-of-arguments)

You may need to specify the order in which the functions associated with a particular hook are executed. Use `piority` option, lower number = higher piority.

Some hooks can pass more than one parameters to your callbacks. You can define it with `numberOfArguments` option.

```php
Hook::action($name, $callback, [
    'piority' => 10,
    'numberOfArguments' => 1
]);
```

<a name="basics-of-hooking"></a>
## [Basics of Hooking](#basics-of-hooking)

Hook creation itself is not enough. After constructing, each hook must make some operation. It can be dispached, detached or performed.

<a name="dispaching-hooks"></a>
### [Dispaching Hooks](#dispaching-hooks)

Delegate hook to perform at the appropriate time with `dispatch` method.

```php
Hook::action('name', $callback)->dispatch();

Hook::filter('name', $callback)->dispatch();
```

<a name="performing-hooks"></a>
### [Performing Hooks](#performing-hooks)

Call `perform` method for executing hook callback.

```php
Hook::action('name')->perform();

Hook::filter('name')->perform();
```

<a name="removing-hooks"></a>
### [Detaching Hooks](#removing-hooks)

Calling `detach` method will remove previously dispached hook.

```php
Hook::action('name', $callback)->detach();

Hook::filter('name', $callback)->detach();
```
