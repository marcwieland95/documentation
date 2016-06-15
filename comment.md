- [Introduction](#introduction)
- [Columns on the list of Comments](#columns-on-the-list-of-comments)
- [Using Comments Repository](#using-comment-repository)
    + [Quering Comments](#quering-comments)
    + [Inserting Comments](#inserting-comments)
    + [Updating Comments](#updating-comments)
    + [Deleting Comments](#deleting-comments)
    + [Displaying Comments In A View](#displaying-comments-in-a-view)


<a name="introduction"></a>
## [Introduction](#introduction)

The Assely framework comes with `App\Comments` repository class that allows for managing WordPress comments.

This singularity, unlike other, is registered by default inside `App\Providers\SingularityServiceProvider` class.

<a name="columns-on-the-list-of-comments"></a>
## [Columns on the list of Comments](#columns-on-the-list-of-comments)

The `columns` method inside `app\Comments.php` file allows to define custom columns which are displayed on the list of comments.

Comments accepts one kind of column: metabox.

```php
public function columns()
{
    return [
        Column::metabox('App\Metaboxes\CommentDetails', 'helpful-flag'),
    ];
}
```

[alert type="info"]More accurate description about `Column` facade you can find in [column documentation](/docs/column).[/alert]

<a name="using-comments-repository"></a>
## [Using Comments Repository](#using-comments-repository)

`Comment` facade provides access to the comments. However, you can also inject the `App\Comments` repository class.

```php
use App\Comments;

public function index(Comments $comments) {
    $comment = $comments->find(1);
}
```

<a name="quering-comment"></a>
### [Quering Comment](#quering-comment)

In this section you will learn how you can query comments in your application.

#### Fetching multiple Comments

The `all` method will retrive all of the comments. This method returns an instance of `Illuminate\Support\Collection`, so you can easily modify results with diffrent [helper methods](https://laravel.com/docs/5.2/collections#available-methods).

```php
$comments = Comment::all();
```

#### Retrieving single Comment

You can also immediately search for single comment. Pass comment id as the `find` method argument.

```php
$comment = Comment::find(1);
```

##### Not Found Query Exception

Sometimes you want to throw an exception when comment was not found. The `findOrFail` method returns found comment, but if not `Assely\Database\QueryException` will be thrown.

```php
Comment::findOrFail(1);
```

#### Custom Query

Needs something more complex? You can use `query` method with array of [ parameters](https://codex.wordpress.org/Class_Reference/WP_Comment_Query#Default_Usage) as argument.

```php
Comment::query([
    'author_email' => 'john.smith@example.com',
]);
```

<a name="inserting-comments"></a>
### [Inserting Comments](#inserting-comments)

Use `create` method with array of properties and values for inserting new comment to the database.

```php
Comment::create([
    'author' => 'john.smith@example.com',
]);
```

##### Inserting Query Exception

The `createdOrFail` method will insert new comment, but when error occurs `Assely\Database\QueryException` will be thrown.

```php
Comment::createOrFail([
    'author' => 'john.smith@example.com',
]);
```

<a name="updating-comments"></a>
### [Updating Comments](#updating-comments)

To update a comment, you need to set new property values on retrived `Assely\Adapter\Comment` instance, and then call `save` method.

```php
$comment = Comment::find(1);

$comment->author = 'John Smith';

$comment->save();
```

<a name="deleting-comments"></a>
### [Deleting Comment](#deleting-comments)

Call `destroy` method on retrieved `Assely\Adapter\Comment` instance to remove comment from database.

```php
$comment = Comment::find(1);

$comment->destroy();
```

<a name="displaying-comments-in-a-view"></a>
### [Displaying comments in a view](#displaying-comments-in-a-view)

You have access to bunch of diffrent properties. List of all you can find in [adapters documentation](/docs/adapters#comment).

```html
<div class="comment">
    <span>{{ $comment->author }}</span>
    <p>{{ $comment->content }}</p>
</div>
```

#### Rendering Comment replies

```html
@foreach($comment->replies() as $reply)
    <span>{{ $reply->author }}</span>
    <p>{{ $reply->content }}</p>
@endforeach
```

#### Utilizing view templates from `resources/views/comments`

Every Assely installation comes with ready to use comments disqusion, replies and form views. This way you have full control over all elements. No more messy `comments_form()` function.

Pass to the view current post instance.

```php
View::make('post', [
    'post' => Post::find(1)
]);
```

Inside view include `resources/views/comments/disqusion.blade.php` template with post id and replies.

```html
@include('comments.disqusion', [
    'id' => $post->id,
    'comments' => $post->comments
]);
```