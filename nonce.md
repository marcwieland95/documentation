- [Introduction](#introduction)
- [Basics of Nonces](#basics-of-nonces)
    + [Creating Nonces](#creating-nonces)
    + [Verifying Nonces](#verifying-nonces)
    + [Outputing Nonces Inputs](#outputing-nonces-inputs)


<a name="introduction"></a>
## [Introduction](#introduction)

[Nonces](https://codex.wordpress.org/WordPress_Nonces) helps protect your actions form misuses by generating and verifying tokens.

Nonces are used to verify if the person, who's doing a specific operation, are entitled to do so. It identifies operiation beetween requests.

<a name="basics-of-nonces"></a>
## [Basics of Nonces](#basics-of-nonces)

Injecting the `Assely\Nonce\NonceFactory` provides access to the nonce service. However, you can also use `Nonce` facade.

```php
use Assely\Nonce\NonceFactory;

public function __construct(NonceFactory $nonce) {
    $this->token = $nonce->create('key');
}
```

<a name="creating-nonces"></a>
### [Creating Nonces](#creating-nonces)

Calling `create` method will return generated token.

```php
$token = Nonce::create($slug);
```

You have to pass this token further and verify before finalizing the operation. Usually this token is transfered as part of request, as url query parameter or form hidden input.

<a name="verifying-nonces"></a>
### [Verifying Nonces](#verifying-nonces)

You can validate given token with `verify` method.

```php
if (Nonce::verify($token)) {
    // token is valid
}
```

<a name="outputing-nonces-inputs"></a>
### [Outputing Nonce Inputs](#outputing-nonces-inputs)

If you need to add nonce inputs to your form there is `fields` method that will help you. It outputs html inputs markup with already generated tokens.

```php
Nonce::fields('token-key');
```

Might echo something like:

```html
<input type="hidden" id="_token-key-nonce" name="_token-key-nonce" value="b192fc4204" />
<input type="hidden" name="_wp_http_referer" value="resource/url/path" />
```
