# wire:text

`wire:text` is a directive that dynamically updates an element's text content based on a component property or expression. Unlike using template output syntax, `wire:text` updates the content without requiring a network roundtrip to re-render the component.

If you are familiar with Alpine's `x-text` directive, the two are essentially the same.

## Basic Usage

Here's an example of using `wire:text` to optimistically show updates to a Livewire property without waiting for a network roundtrip.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ShowPost.bx
class extends="cbwire.models.Component" {
    data = {
        "postId": 0,
        "likes": 0
    };

    function onMount( params ) {
        data.postId = params.postId;

        var post = getInstance("Post").findOrFail(data.postId);
        data.likes = post.getLikeCount();
    }

    function like() {
        var post = getInstance("Post").findOrFail(data.postId);
        post.like();

        data.likes = post.fresh().getLikeCount();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ShowPost.cfc
component extends="cbwire.models.Component" {
    data = {
        "postId" = 0,
        "likes" = 0
    };

    function onMount( params ) {
        data.postId = params.postId;

        var post = getInstance("Post").findOrFail(data.postId);
        data.likes = post.getLikeCount();
    }

    function like() {
        var post = getInstance("Post").findOrFail(data.postId);
        post.like();

        data.likes = post.fresh().getLikeCount();
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/showPost.bxm -->
<bx:output>
<div>
    <button x-on:click="$wire.likes++" wire:click="like">❤️ Like</button>

    Likes: <span wire:text="likes"></span>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/showPost.cfm -->
<cfoutput>
<div>
    <button x-on:click="$wire.likes++" wire:click="like">❤️ Like</button>

    Likes: <span wire:text="likes"></span>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

When the button is clicked, `$wire.likes++` immediately updates the displayed count through `wire:text`, while `wire:click="like"` persists the change to the database in the background.

This pattern makes `wire:text` perfect for building optimistic UIs in Livewire.
