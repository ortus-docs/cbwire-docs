# Events

## Overview

Events and listeners provide an elegant means for your components to communicate. You can dispatch events from one component, listen for them on another, execute actions, and re-render the listener.

## Dispatching Events

You can dispatch events directly in your component [actions](actions.md) using **dispatch()**.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Posts.bx
class extends="cbwire.models.Component" {
    function addPost() {
        dispatch( "post-added" );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Posts.cfc
component extends="cbwire.models.Component" {
    function addPost() {
        dispatch( "post-added" );
    }
}
```
{% endtab %}
{% endtabs %}

You can dispatch events from your [templates](templates.md) using **$dispatch()**.

```html
<!--- ./wires/posts.bxm|cfm --->
<div>
    <button wire:click="$dispatch( 'post-added' )">Add Post</button>
</div>
```

You can also dispatch using the [Livewire JavaScript object](javascript.md#livewire-object).

```html
<!--- ./wires/posts.bxm|cfm --->
<script>
    Livewire.dispatch( 'post-added' );
</script>
```

## Passing Parameters

You can send parameters as your dispatching an event.

```javascript
function addPost() {
    dispatch( "post-added", { "id": post.getId() } );
}
```

```html
<button wire:click="$dispatch( 'post-added', { id: post.getId() } )">Add Post</button>
```

```html
<script>
    Livewire.dispatch( 'post-added', { id: '#post.getID()#' } );
</script>
```

## dispatchTo

You can dispatch events to components of a specific component using **dispatchTo()**.

```javascript
function addPost() {
    dispatchTo( "TopBar", "post-added", {} );
}
```

```html
<button wire:click="$dispatchTo( 'TopBar', 'post-added', {} )">Add Post</button>
```

```html
<script>
    Livewire.dispatchTo( 'TopBar', 'post-added', { id: '#post.getID()#' } );
</script>
```

## dispatchSelf

You can dispatch events to the component that fired the event using **dispatchSelf(**).

```javascript
function addPost() {
    dispatchSelf( "post-added", { "id": post.getId() } );
}
```

```html
<button wire:click="$dispatchSelf( 'post-added', { id: post.getId() } )">Add Post</button>
```

## Listeners

You register event listeners by adding a **listeners** struct to your component.

Listeners are a key/value pair where the key is the event to listen for, and the value is the action to invoke on the component.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Posts.bx
class extends="cbwire.models.Component" {
    listeners = {
        "post-added": "incrementPostCount" 
    };   
    function incrementPostCount() {
        // Increment the post count
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Posts.cfc
component extends="cbwire.models.Component" {
    listeners = {
        "post-added": "incrementPostCount" 
    };   
    function incrementPostCount() {
        // Increment the post count
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
JavaScript keys are case-sensitive. To preserve the key casing in CFML, surround your listener names in quotations.
{% endhint %}

## Dispatching Browser Events

You can also dispatch browser events using the **dispatch()** method.

```javascript
function submitForm(){
    // Dispatch a 'form-submitted' browser event. Pass parameters
    dispatch( "form-submitted", { "submittedBy": "Grant" } );
}
```

```html
<div>
    <form wire:submit.prevent="submitForm">
        <button type="submit">Dispatch event</button>
    </form>
</div>
```

You can listen for the event using vanilla JavaScript:

```html
<script>
    window.addEventListener('form-submitted', event => {
        alert( 'Form submitted by: ' + event.detail.submittedBy );
    } );
</script>
```

If you use [Alpine.js](javascript.md#alpine.js), you can listen for an event like this.

```html
<div x-data="{ show: false }" @form-submitted.window="show = true">
    <div x-show="true">
        Form was submitted.
    </div>
</div>
```
