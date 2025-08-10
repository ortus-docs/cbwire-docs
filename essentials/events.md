---
description: A simple way for UI components to communicate with each other.
---

# Events

Events and listeners provide an elegant means for your components to communicate with each other. You can emit events from one component, listen for them on another component, and then execute code and re-render the listener.

## Emitting Events

You can fire events from:

* Actions using **emit(), emitUp(), emitSelf(),** and **emitTo()**
* Templates using **$emit()**
* [JavaScript](javascript.md) using **cbwire.emit()**

```html
<!--- Action --->
<cfscript>
    function addPost() {
        emit( "postAdded" );
    }
</cfscript>
```

```html
<!--- Templates --->
<cfoutput>
    <div>
        <button wire:click="$emit( 'postAdded' )">Add Post</button>
    </div>
</cfoutput>

```

```html
<!--- Javascript --->
<script>
    cbwire.emit( 'postAdded' );
</script>
```

## Passing Parameters

You can send parameters as your emitting an event.

```javascript
function addPost() {
    var post = getInstance( "PostService" ).create( data );
    emit( "postAdded", post.getId() );
}

<button wire:click="$emit( 'postAdded', post.getId() )">Add Post</button>

<script>
    cbwire.emit( 'postAdded', [ '#post.getID()#' ] );
</script>
```

## Scoping Events

### emitUp

When working with nested components, sometimes you may want to emit events only to parent components and no children or sibling components.

For this, you can use the **emitUp()** method.

```html
<cfoutput>
    <div>...</div>
</cfoutput>

<cfscript>
    function addPost() {
        emitUp( "postAdded" );
    }
</cfscript>
```

```html
<button wire:click="$emitUp( 'postAdded' )">Add Post</button>
```

### emitTo

You can emit events to components of a specific type ( name ) using **emitTo()**.

```html
<cfoutput>
    <div>...</div>
</cfoutput>

<cfscript>
    function addPost() {
        emitTo( "topBar", "postAdded" );
    }
</cfscript>
```

```html
<button wire:click="$emitTo( 'topBar', 'postAdded' )">Add Post</button>
```

### emitSelf

You can emit events to the component that fired the event using **emitSelf()**.

```html
<cfoutput>
    <div>...</div>
</cfoutput>

<cfscript>
    function addPost() {
        emitSelf( "postAdded" );
    }
</cfscript>
```

```html
<button wire:click="$emitSelf( 'postAdded' )">Add Post</button>
```

## Listeners

You register event listeners using the **listeners** struct of your component.

Listeners are a key/value pair where the key is the event to listen for, and the value is the method to invoke on the component.

```html
<cfscript>
    listeners = {
        "postAdded": "incrementPostCount" 
    };
    
    function incrementPostCount() {
        // Increment the post count
    }
</cfscript>
```

{% hint style="info" %}
JavaScript keys are case-sensitive. You can preserve the key casing in CFML by surrounding your listener names in quotations.
{% endhint %}

## Dispatching Browser Events

You can dispatch browser events using the **dispatch()** method.

```html
// wires/MyComponent.cfm
<cfscript>
    data = {};

    function submitForm(){
        // Dispatch a 'form-submitted' browser event. Pass parameters
        dispatch( "form-submitted", { "submittedBy": "Grant" } );
    }
</cfscript>

<cfoutput>
    <div>
        <form wire:submit.prevent="submitForm">
            <button type="submit">Dispatch event</button>
        </form>
    </div>
</cfoutput>

```

You can listen for the event using vanilla JavaScript:

```html
<script>
    window.addEventListener('form-submitted', event => {
        alert('Form submitted by: ' + event.detail.submittedBy);
    } );
</script>
```

If you are using AlpineJS, you can listen for the event like this.

```html
<div x-data="{ show: false }" @form-submitted.window="show = true">
    <div x-show="true">
        Form was submitted.
    </div>
</div>
```
