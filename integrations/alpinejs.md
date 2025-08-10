---
description: >-
  Alpine JS brings simple client-side reactivity when you need it and integrates
  beautifully with CBWIRE.
---

# AlpineJS

Many page interactions don't warrant a full server roundtrip, such as toggling a modal or a hidden element. In these instances, we recommend using [AlpineJS](https://alpinejs.dev/).

{% hint style="info" %}
AlpineJS allows you to add JavaScript behavior directly into your markup in a declarative way. If you are familiar with VueJS, Alpine should feel similar.
{% endhint %}

## Installation

```html
<head>
    <script src="//unpkg.com/alpinejs" defer></script>
    <!-- The "defer" attribute is important to ensure Alpine waits for CBWIRE to load first. -->
</head>
```

For more installation information, visit [Alpine Docs](https://alpinejs.dev/).

## Templates

Below is an example of using [AlpineJS](https://alpinejs.dev/) to toggle a list on the page.

```html
<div>
    <div x-data="{ open: false }">
        <button @click="open = true">Show More...</button>
 
        <ul x-show="open" @click.away="open = false">
            <li><button wire:click="archive">Archive</button></li>
            <li><button wire:click="delete">Delete</button></li>
        </ul>
    </div>
</div>
```

## Shared State

CBWIRE has a powerful _entangle()_ method that allows you to "entangle" a CBWIRE and AlpineJS data property. With entanglement, both client-side and server-side properties are instantly synchronized, regardless of whether the value was changed in CFML or client-side using JavaScript.

{% hint style="warning" %}
You need CBWIRE v2.3.6 or greater to use _entangle()_.
{% endhint %}

{% hint style="success" %}
This provides data model binding both client-side and server-side.
{% endhint %}

Consider this simple Counter component:

```html
<!--- ./wires/Counter.cfm --->
<cfscript>
    data = {
        "counter": 0
    };

    function increment() {
        data.counter += 1;
    }
</cfscript>

<cfoutput>
    <div x-data="{ counter: #entangle( 'counter' )# }">
        <div>CBWIRE value: #counter#</div>
        <div>AlpineJS value: <span x-html="counter"></span></div>
        
        <button
            wire:click="increment"
            type="button">Increment with CBWIRE</button>
        
        <button
            @click="counter += 1"
            type="button">Increment with AlpineJS</button>
    </div>
</cfoutput>
```

We define an AlpineJS property named _counter and_ then call the built-in CBWIRE method _entangle()_, passing it the name of the server-side data property we want to bind with.

```html
<div x-data="{ counter: #entangle( 'counter' )# }">
```

Next, we are incrementing our Counter in two separate ways:

* Incrementing the counter by calling the _increment()_ action using CBWIRE
* Incrementing the AlpineJS property _counter_ in JavaScript, triggering an immediate update to the server and re-rendering of the component.

```html
<button wire:click="increment" type="button">Increment with CBWIRE</button>
<button @click="counter += 1" type="button">Increment with AlpineJS</button>
```

Updating CBWIRE server-side on every AlpineJS property change is optional. You can also delay the server-side updates until the next CBWIRE request that goes out by chaining a _.defer_ modifier like so.

```html
<div x-data="{ counter: #entangle( 'counter' )#.defer }">
```
