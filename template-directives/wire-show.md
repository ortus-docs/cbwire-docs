# wire:show

Livewire's `wire:show` directive makes it easy to show and hide elements based on the result of an expression.

The `wire:show` directive is different than using conditionals in your template in that it toggles an element's visibility using CSS (`display: none`) rather than removing the element from the DOM entirely. This means the element remains in the page but is hidden, allowing for smoother transitions without requiring a server round-trip.

## Basic Usage

Here's a practical example of using `wire:show` to toggle a "Create Post" modal:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/CreatePost.bx
class extends="cbwire.models.Component" {
    data = {
        "showModal": false,
        "content": ""
    };

    function save() {
        getInstance("Post").create({ "content": data.content });

        reset("content");

        data.showModal = false;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/CreatePost.cfc
component extends="cbwire.models.Component" {
    data = {
        "showModal" = false,
        "content" = ""
    };

    function save() {
        getInstance("Post").create({ "content" = data.content });

        reset("content");

        data.showModal = false;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/createPost.bxm -->
<bx:output>
<div>
    <button x-on:click="$wire.showModal = true">New Post</button>

    <div wire:show="showModal">
        <form wire:submit="save">
            <textarea wire:model="content"></textarea>

            <button type="submit">Save Post</button>
        </form>
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/createPost.cfm -->
<cfoutput>
<div>
    <button x-on:click="$wire.showModal = true">New Post</button>

    <div wire:show="showModal">
        <form wire:submit="save">
            <textarea wire:model="content"></textarea>

            <button type="submit">Save Post</button>
        </form>
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

When the "New Post" button is clicked, the modal appears without a server roundtrip. After successfully saving the post, the modal is hidden and the form is reset.

## Using Transitions

You can combine `wire:show` with Alpine.js transitions to create smooth show/hide animations. Since `wire:show` only toggles the CSS display property, Alpine's `x-transition` directives work perfectly with it:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/createPost.bxm -->
<bx:output>
<div>
    <button x-on:click="$wire.showModal = true">New Post</button>

    <div wire:show="showModal" x-transition.duration.500ms>
        <form wire:submit="save">
            <textarea wire:model="content"></textarea>
            <button type="submit">Save Post</button>
        </form>
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/createPost.cfm -->
<cfoutput>
<div>
    <button x-on:click="$wire.showModal = true">New Post</button>

    <div wire:show="showModal" x-transition.duration.500ms>
        <form wire:submit="save">
            <textarea wire:model="content"></textarea>
            <button type="submit">Save Post</button>
        </form>
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

The Alpine.js transition classes above will create a fade and scale effect when the modal shows and hides.

{% hint style="info" %}
For more information on Alpine.js transitions, visit the [Alpine.js x-transition documentation](https://alpinejs.dev/directives/transition).
{% endhint %}
