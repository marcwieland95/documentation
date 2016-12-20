- [Introduction](#introduction)
- [Adapter's properties](#adapters-properties)
    + [Post Adaptee](#post-adaptee)
    + [Term Adaptee](#term-adaptee)
    + [Menu Adaptee](#menu-adaptee)
    + [User Adaptee](#user-adaptee)
    + [Comment Adaptee](#comment-adaptee)
- [Serializing Adapters](#serializing-adapters)
    + [Serializing to Array](#serializing-to-array)
    + [Serializing to JSON](#serializing-to-json)
- [Accessing Original Adaptee](#accessing-original-adaptee)

<a name="introduction"></a>
## [Introduction](#introduction)

The Assely framework introduces Adapters for one important reason — to unify and enhance WordPress core objects. You will have access to various methods previously unavailable directly on core objects.

<a name="adapters-properties"></a>
## [Adapter's properties](#adapters-properties)

Do you remember how often you had var_dump to recall under what property name this `id` of object is? Once it is `id`, some other time `ID`, even `term_id` and `comment_ID`. What a mess. That is one of the tiniest, but most annoing things. Now, you can forget about it.

<a name="post-adapter"></a>
### [Post Adaptee](#post-adaptee)

This adapter represents WordPress post objects.

#### List of available properties

| Property | Alias for | Description |
|---|---|---|
| `author` | post_author | Post author |
| `comment_count` | comment_count | Count of post comments |
| `comment_status` | comment_status | Post comments open status |
| `comments` | --- | Post comments. |
| `content` | post_content | Post content |
| `created_at` | post_date | Post creation date |
| `excerpt` | post_excerpt | Post excerpt |
| `format` | --- | Post format |
| `id` | ID | Post id |
| `link` | --- | The post link |
| `menu_order` | menu_order | Order of post in menu |
| `meta` | --- | Post metadata |
| `mime_type` | post_mime_type | Mime type of post |
| `modified_at` | post_modified | Post modification date |
| `parent_id` | post_parent | Post parent id |
| `password` | post_password | Post password |
| `ping_status` | ping_status | Post ping open status |
| `ping` | to_ping | Post ping |
| `pinged` | pinged | Post pinged |
| `slug` | post_name | Post slug |
| `status` | post_status | Status of post |
| `template` | --- | Post custom template |
| `terms` | --- | Post terms. |
| `thumbnail` | --- | Post thumbnail image |
| `title` | post_title | Post title |
| `type` | post_type | Type of post |

<a name="term-adapter"></a>
### [Term Adaptee](#term-adaptee)

This adapter represents WordPress term objects.

#### List of available properties

| Property | Alias for | Description |
|---|---|---|
| `count` | count | Cached object count for term |
| `description` | description | Term description |
| `group` | term_group | Term group |
| `id` | term_id | Term id |
| `meta` | --- | Term metadata |
| `parent_id` | parent | Id of a term's parent term |
| `posts` | --- | Collection of term's posts |
| `slug` | slug | Term slug |
| `taxonomy_id` | term_taxonomy_id | Id of a term's taxonomy |
| `taxonomy_slug` | taxonomy | Slug of a term's taxonomy |
| `title` | name | Term title |

<a name="menu-adapter"></a>
### [Menu Adaptee](#menu-adaptee)

This adapter represents WordPress menu entries.

#### List of available properties

| Property | Alias for | Description |
|---|---|---|
| `attr` | attr_title | Entry title attribute |
| `children` | --- | Collection of children entries |
| `description` | description | Entry description |
| `id` | ID | Entry id |
| `item_id` | object_id | Id of entry's item object |
| `item_type` | type | Type of entry's item object |
| `link` | url | Entry url |
| `modified_at` | post_modified | Entry modification date |
| `order` | menu_order | Entry order |
| `parent_id` | menu_item_parent | Entry's parent id |
| `target` | target | Entry target attribute |
| `title` | title | Entry title |

<a name="user-adapter"></a>
### [User Adaptee](#user-adaptee)

This adapter represents WordPress user objects.

#### List of available properties

| Property | Alias for | Description |
|---|---|---|
| `activation_key` | user_activation_key | User activation key |
| `capabilities` | caps | List of user capabilities |
| `capability_key` | cap_key | --- |
| `created_at` | user_registered | User registration date |
| `email` | user_email | User email |
| `id` | ID | User id |
| `login` | user_login | User login |
| `meta` | --- | User metadata |
| `name` | display_name | User name |
| `password` | user_pass | User password |
| `premissions` | allcaps | List of user premissions |
| `roles` | roles | User asigned roles |
| `status` | user_status | User activity status |
| `username` | user_nicename | User username |
| `website` | user_url | User website url |

<a name="comment-adapter"></a>
### [Comment Adaptee](#comment-adaptee)

This adapter represents WordPress comment objects.

#### List of available properties

| Property | Alias for | Description |
|---|---|---|
| `agent` | comment_agent | Comment's agents details |
| `approved` | comment_approved | Status of the comment's approbation |
| `author_email` | comment_author_email | Comment author's email |
| `author_ip` | comment_author_IP | Comment autor's ip |
| `author_url` | comment_author_url | Comment author's website url |
| `author` | comment_author | Comment author's name |
| `content` | comment_content | Content of the comment |
| `created_at` | comment_date | Comment creation date |
| `id` | comment_ID | Comment id |
| `karma` | comment_karma | Comment karma ranking |
| `parent_id` | comment_parent | Parent comment id |
| `post_id` | comment_post_ID | Comment's post id |
| `type` | comment_type | Type of comment (null, pingback, trackback) |
| `user_id` | user_id | Id of user, that added the comment |

<a name="serializing-adapters"></a>
## [Serializing Adapters](#serializing-adapters)

Adapters are easily convertible to Array and JSON. It's useful especially when you want to pass your objects to the frontend.

<a name="serializing-to-array"></a>
### [Serializing to Array](#serializing-to-array)

The `toArray` method converts the adapter instance into a standard PHP `array`.

```php
Post::find(1)->toArray();

/*
[
    'id' => 1,
    'parent' => 0,
    'created_at' => '2016-06-15 20:22:08',
    'modified_at' => '2016-06-19 08:20:18',
    (...)
]
*/
```

<a name="serializing-to-json"></a>
### [Serializing to JSON](#serializing-to-json)

The `toJson` method converts the adapter instance into a JSON.

```php
Post::find(1)->toJson();

/*
{
    id: '1',
    parent: '0',
    created_at: '2016-06-15 20:22:08',
    modified_at: '2016-06-19 08:20:18',
    (...)
}
*/
```

<a name="accessing-original-adaptee"></a>
## [Accessing Original Adaptee](#accessing-original-adaptee)

There are some situations where you need to process source object. In this cases, you can simply get originally adapted object with `getAdaptee` getter.

```php
// Gets the WP_Post object
Post::find(1)->getAdaptee();
```