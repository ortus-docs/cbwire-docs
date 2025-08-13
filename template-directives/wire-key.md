# wire:key

Using **wire:key** is essential, especially when looping over elements, to ensure that Livewire's DOM diffing correctly identifies what has changed and needs updating in the browser.

Consider this listing of posts.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Posts.bx
class extends="cbwire.models.Component" {
    function posts() {
        return queryExecute( "select id,title from posts" );
    }
    function remove( postId ) {
        queryExecute( "delete from posts where id = :id", { id = postId } );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Posts.cfc
component extends="cbwire.models.Component" {
    function posts() {
        return queryExecute( "select id,title from posts" );
    }
    function remove( postId ) {
        queryExecute( "delete from posts where id = :id", { id = postId } );
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/posts.bxm --->
<div>
    <bx:loop array="#posts()#" index="post">
        <div>
            <h2>#post.title#</h2>
            <button wire:click="remove( #post.id# )">Remove</button>
        </div>
    </bx:loop>
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/posts.cfm --->
<div>
    <cfloop array="#posts()#" index="post">
        <div>
            <h2>#post.title#</h2>
            <button wire:click="remove( #post.id# )">Remove</button>
        </div>
    </cfloop>
</div>
```
{% endtab %}
{% endtabs %}

As posts are removed, CBWIRE re-renders our template, and then Livewire must figure out how to update the DOM. **We can help Livewire know exactly what has changed using wire:key.**

```html
<div wire:key="post-#post.id#">
    <h2>#post.title</h2>
    ....
</div>
```

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/posts.bxm --->
<div>
    <bx:loop array="#posts()#" index="post">
        <div wire:key="post-#post.id#">
            <h2>#post.title#</h2>
            <button wire:click="remove( #post.id# )">Remove</button>
        </div>
    </bx:loop>
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/posts.cfm --->
<div>
    <cfloop array="#posts()#" index="post">
        <div wire:key="post-#post.id#">
            <h2>#post.title#</h2>
            <button wire:click="remove( #post.id# )">Remove</button>
        </div>
    </cfloop>
</div>
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
It's important to ensure your key names are unique throughout the page, not just within your template. Otherwise, you may encounter weird rendering issues.
{% endhint %}
