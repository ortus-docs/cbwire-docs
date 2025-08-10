---
description: Beautifully integrate your client-side JavaScript and CBWIRE using AlpineJS.
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

For more installation information, visit the [Alpine Docs](https://alpinejs.dev/).

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

## Entangle: Share State Between CBWIRE And AlpineJS

{% hint style="warning" %}
You need CBWIRE v2.3.6 or greater to use entangle.
{% endhint %}

CBWIRE has a powerful _entangle()_ method that allows you to "entangle" a CBWIRE and AlpineJS data property. With entanglement, both client-side and server-side properties are instantly synchronized, regardless of whether the value was changed server-side in CFML or client-side using JavaScript.

{% hint style="success" %}
This provides data model binding both client-side and server-side.
{% endhint %}

Consider this simple Counter [Wire](../essentials/creating-components.md):

```javascript
// wires/Counter.cfc
component extends="cbwire.models.Component" {

    data = {
        "counter": 0
    };

    function increment() {
        data.counter += 1;
    }
}
```

And now, our template:

<pre class="language-html"><code class="lang-html"><strong>&#x3C;!--- views/wires/counter.cfm --->
</strong><strong>&#x3C;cfoutput>
</strong>    &#x3C;div x-data="{ counter: #entangle( 'counter' )# }">
        &#x3C;div>CBWIRE value: #args.counter#&#x3C;/div>
        &#x3C;div>AlpineJS value: &#x3C;span x-html="counter">&#x3C;/span>&#x3C;/div>
        
        &#x3C;button
            wire:click="increment"
            type="button">Increment with CBWIRE&#x3C;/button>
        
        &#x3C;button
            @click="counter += 1"
            type="button">Increment with AlpineJS&#x3C;/button>
    &#x3C;/div>
&#x3C;/cfoutput>
</code></pre>

We define an AlpineJS property named _counter and_ then call the built-in CBWIRE method _entangle()_, passing it the name of the server-side data property we want to bind with.

```html
<div x-data="{ counter: #entangle( 'counter' )# }">
```

Next, we are incrementing our Counter in two separate ways:

* Incrementing the counter by calling the increment [Action](../essentials/actions.md) using CBWIRE
* Incrementing the AlpineJS property _counter_ value in JavaScript, triggering an immediate update to the server and re-rendering of the Counter [Wire](../essentials/creating-components.md).

```html
<button wire:click="increment" type="button">Increment with CBWIRE</button>
<button @click="counter += 1" type="button">Increment with AlpineJS</button>
```

Updating CBWIRE server-side on every AlpineJS property change is optional. You can also delay the server-side updates until the next CBWIRE request that goes out by chaining a _.defer_ modifier like so.

```html
<div x-data="{ counter: #entangle( 'counter' )#.defer }">
```
