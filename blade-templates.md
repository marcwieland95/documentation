- [Introduction](#introduction)
- [WordPress Directives](#wordpress-directives)
    + [Head Action](#head-action)
    + [Footer Action](#footer-action)
    + [Cache Block](#cache-block)
    + [Widget Block](#widget-block)


<a name="introduction"></a>
## [Introduction](#introduction)

The Assely framework fully utilizes Laravel Blade Templates Engine. Please visit official [documentation](https://laravel.com/docs/5.3/blade) for a comprehensive descriptions.

<a name="wordpress-directives"></a>
## [WordPress Directives](#wordpress-directives)

Assely framework brings few ready to use Blade Directives that facilitate some of WordPress standard templates functions.

<a name="head-action"></a>
### [Head Action](#head-action)

```html
<head>
    <!-- ... head elements -->

    @wphead()
</head>
```

Directive for [wp_head](https://codex.wordpress.org/Function_Reference/wp_head). Always have this directive just before the closing </head> tag of your theme, or you will break many plugins.

<a name="footer-action"></a>
### [Footer Action](#footer-action)

```html
<body>
    <!-- ... body elements -->

    @wpfooter()
</body>
```

Directive for [wp_footer](https://codex.wordpress.org/Function_Reference/wp_footer). Always have this directive just before the closing </body> tag of your theme, or you will break many plugins.

<a name="cache-block"></a>
### [Cache Block](#cache-block)

```html
@cache($key, $arguments)
    {{-- content --}}
@endcache
```

It caching content of the directive. Read detailed description in [Cache chapter](/docs/cache#cache-blade-directive).

<a name="eval-shortcodes"></a>
### [Eval Shorcodes](#eval-shortcodes)

```html
@shortcodes()
    [my_shortcode]
@endshortcodes
```

All shortcodes present inside this block will be evaluated.