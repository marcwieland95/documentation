- [Introduction](#introduction)
- [Creating Profile](#creating-profile)
    + [Via console command](#via-console-command)
    + [Manually created file](#manually-created-file)
- [Configuring Profile](#creating-profile)
    + [Registering Profile](#registering-profile)
    + [Publishing Profiles for Users](#publishing-profiles-for-users)
    + [The Arguments Method](#the-arguments-method)
    + [The Relation Method](#the-relation-method)
    + [The Fields Method](#the-fields-method)


<a name="introduction"></a>
## [Introduction](#introduction)

[alert type="info"]Profiles are a part of singularities. I encourage you to read general [Singularities](/docs/singularities) documentation chapter.[/alert]

Profiles allows you to display custom fields in your users profiles.

<a name="creating-profile"></a>
## [Creating Profile](#creating-profile)

By default, profiles are stored inside `app\Profiles` directory.

### Via console command

Scafford profile with `wp assely:make profile` command.

```bash
wp assely:make profile SocialProfiles
```

##### Specifying slug

This will create profile with `socialprofiles` slug, but you can specify it with `--slug` option.

```bash
wp assely:make profile SocialProfiles --slug="social-profiles"
```

##### Specifying owners

You can also define to which singularity this profile belongs to. Enter it's classname to the `--belongsto` option.

```bash
wp assely:make profile SocialProfiles --belongsto="App\Users"
```

### Manually created class file

Make file with class that extends base `Assely\Profile\Profile` class.

The `slug` property is required. This value determines under which name your metadata will be registered and accessible.

```php
// app/Profiles/SocialProfiles.php

namespace App\Profiles;

use Assely\Profile\Profile;

class SocialProfiles extends Profile
{
    /**
     * Profile slug.
     *
     * @var string
     */
    public $slug = 'social-profiles';

    /**
     * Describe profile relationships.
     *
     * @return self
     */
    public function relation()
    {
        return $this->belongsTo(['App\Users']);
    }

    /**
     * Profile arguments.
     *
     * @return array
     */
    public function arguments()
    {
        return [
            //
        ];
    }

    /**
     * Register profile custom fields.
     *
     * @return \Assely\Field\Field[]
     */
    public function fields()
    {
        return [
            //
        ];
    }
}
```

<a name="configuring-profile"></a>
## [Configuring Profile](#configuring-profile)

<a name="registering-profile"></a>
### [Registering Profile](#registering-profile)

All profiles are registered in the `config/app.php` configuration file. This file contains a `profiles` array where you can add your newly created users profiles.

```php
'profiles' => [
    // ...other profiles

    App\Profiles\SocialProfiles::class,
],
```

<a name="publishing-profiles-for-users"></a>
### [Publishing Profiles for Users](#publishing-profiles-for-users)

By default, profile fields are private and visible only for administrators, but if you want to allow users to fill the fields themselves you need to change profile visibility to public.

All you have to do is use `Assely\Profile\MakePublic` trait inside your created profile. That is it.

```php
use Assely\Profile\MakePublic;

class SocialProfiles extends Profile
{
    use MakePublic;

    // rest of the profile class...
}
```

<a name="the-arguments-method"></a>
### [The Arguments Method](#the-arguments-method)

The `arguments` method must always return an array of profile registration parameters.

```php
public function arguments()
{
    return [
        'title' => ['Social Profile', 'Social Profiles'],
        'description' => 'Links to the user social profiles.'
    ];
}
```

| Parameter name | Default value | Description |
|---------|---------|---------|
| title | `[]` | Array of titles, where first element is singular variant, second a plural. |
| description | `''` | Description text displayed above all fields. |
| preserve | `'single'` | How profile [preserves his metadata](/docs/singularities#configuring-how-metadata-is-stored). |

<a name="the-relation-method"></a>
### [The Relation Method](#the-relation-method)

Profiles needs to belongs somewhere. The `relation` method allows you to describe where profile should appear. Use `belongsTo` method with array of profile owners as parameter.

```php
public function relation()
{
    return $this->belongsTo(['App\Users']);
}
```

<a name="the-fields-method"></a>
### [The Fields Method](#the-fields-method)

The `fields` method allows to list custom fields which will be managed by profile.

[alert type="info"]More detailed descriptions about fields you can read in [Fielder documentation](/docs/field-types).[/alert]

```php
use Field;

public function fields()
{
    return [
        Field::text('twitter_url')
    ];
}
```
