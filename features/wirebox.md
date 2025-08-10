# WireBox

## Overview

You can access any dependencies your component may have using [WireBox](https://wirebox.ortusbooks.com/), ColdBox's robust dependency injection framework.

{% hint style="success" %}
This is a great way to keep your business logic out of your components.
{% endhint %}

You can achieve this by defining a CFC property in your component and using **inject**.

```javascript
property name="postService" inject="PostService";
```

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./models/PostService.bx
class singleton {
    function getAll() {
        return queryExecute( "select * from posts" );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./models/PostService.cfc
component singleton {
    function getAll() {
        return queryExecute( "select * from posts" );
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Posts.bx
class extends="cbwire.models.Component" {
    property name="postService" inject="PostService";
    function allPosts() {
        return postService.getAll();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Posts.cfc
component extends="cbwire.models.Component" {
    property name="postService" inject="PostService";
    function allPosts() {
        return postService.getAll();
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/posts.bxm --->
<cfoutput>
    <div>
        <h1>Posts</h1>
        <bx:loop array="#allPosts()#" index="post">
            <div wire:key="post-#post.id#">
                <h2>#post.title#</h2>
            </div>
        </bx:loop>
    </div>
</cfoutput>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/posts.cfm --->
<cfoutput>
    <div>
        <h1>Posts</h1>
        <cfloop array="#allPosts()#" index="post">
            <div wire:key="post-#post.id#">
                <h2>#post.title#</h2>
            </div>
        </cfloop>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
CBWIRE uses WireBox internally to load your components. Any lifecycle methods fired by WireBox are available to you.
{% endhint %}

## Getting Instances

You can use **getInstance()** to access a dependency from within your actions.

```javascript
function allPosts() {
    var postService = getInstance( "PostService" );
    return postService.getAll();
}
```

Here is the method signature for **getInstance()**:

```javascript
/**
 * Get a instance object from WireBox
 *
 * @name The mapping name or CFC path or DSL to retrieve
 * @initArguments The constructor structure of arguments to passthrough when initializing the instance
 * @dsl The DSL string to use to retrieve an instance
 *
 * @return The requested instance
 */
function getInstance( name, initArguments={}, dsl )
```

## Documentation

Learn about the full capabilities of WireBox at [https://wirebox.ortusbooks.com/](https://wirebox.ortusbooks.com/).
