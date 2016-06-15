- [Configuration](#configuration)
- [Usage](#usage)
    + [Getting Cache Value](#getting-cache-value)
    + [Putting Cache Value](#putting-cache-value)
    + [Checking Cache Existence](#checking-cache-existence)
    + [Flushing Cache Value](#flushing-cache-value)
- [Cache Blade Directive](#cache-blade-directive)
    + [Nesting Directives](#nesting-directives)
    + [Adapters Caching](#adapters-caching)
    + [Arguments](#arguments)


<a name="configuration"></a>
## [Configuration](#configuration)

All cache related configs are stored inside `config/cache.php` file. All options are documented there, follow comments.

<a name="usage"></a>
## [Usage](#usage)

Simply use the `Cache` facade. However, if you have access to the application service container you can inject `Assely/Cache/Cache` as dependency.

```php
use Assely\Cache\Cache;

public function __construct(Cache $cache) {
    $this->cache = $cache;
}

public function register() {
    $value = $this->cache->get('key');
}
```

<a name="getting-cache-value"></a>
### [Getting Cache Value](#getting-cache-value)

To receive cache value simply call `get` method.

```php
Cache::get('key');
```

<a name="checking-cache-existence"></a>
### [Checking Cache Existence](#checking-cache-existence)

Call `has` method to check if cache with specifed key already exsists.

```php
if (Cache::has('key')) {
    // ...have cache
}
```

<a name="putting-cache-value"></a>
### [Putting Cache Value](#putting-cache-value)

Use `put` method to save value into cache. Third argument is cache expiration time, if this is not determined, it will use default config form `config/cache.php` file.

```php
Cache::put('key', $value, $expire);
```

[alert type="info"]Use [Time_Constants](https://codex.wordpress.org/Easier_Expression_of_Time_Constants) for easier expression of caching expiration time.[/alert]

<a name="flushing-cache-value"></a>
### [Flushing Cache Value](#flushing-cache-value)

For clearing cache call `flush` method with key of cache that has to be flushed.

```php
Cache::flush('key');
```

<a name="cache-blade-directive"></a>
## [Cache Blade Directive](#cache-blade-directive)

You can cache parts of your views by surrounding they with `@cache` and `@endcache` directives. On the next run, if cache is present and not expiried, content of the directive will never run. Instead, will be pulled from the cache.

```html
@cache($key, $arguments)
    {{-- content --}}
@endcache
```

<a name="nesting-blocks"></a>
### [Nesting Blocks](#nesting-blocks)

Cache directives can be nested, but you have to take into account that child cache blocks will not even be able to run until the parent cache expires.

```html
@cache('cache.parent')
    {{-- parent content --}}

    @cache('cache.nested')
        {{-- child content --}}
    @endcache
@endcache
```

<a name="adapters-caching"></a>
### [Adapters Caching](#adapters-caching)

Passing an instance of `Assely\Adapter\Adapter` as key results with timestamp-based caching. This is useful especially when your posts have heavy views (displays metas, terms etc.).

```html
@foreach($movies->all() as $movie)
    @cache($movie)
        {{ $movie->meta('details')->get('release_date') }}
    @endcache
@endforeach
```

[alert type="danger"]Timestamp-based caching requires to integrate some kind of
mechanism for regularly clearing expired cache. You can make it a part of the deployment workflow or setup a cron job. It is all up to you.[/alert]

This approach uses `modified_at` date as part of cache key. On update, this timestamp changes and automaticaly invalidates cache.

Cache keys are constructed in the following way `{classname}/{id}-{modified_at}`. Caching `Assely\Adapter\Post` with id of 1 may produce `Assely\Adapter\Post/1-1452636753` key.

<a name="arguments"></a>
### [Arguments](#arguments)

Default expiration time is configured in `app/cache.php`, but if you want to have diffrent expire time for specifed cache block you can pass it as second argument.

```html
@cache('cache.key', ['expire' => DAY_IN_SECONDS])
    {{-- content --}}
@endcache
```
