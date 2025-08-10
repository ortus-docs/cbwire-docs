---
description: >-
  CBWIRE is a ColdBox module that uses Livewire and Alpine.js to help you build
  modern, reactive BoxLang and CFML applications in record time without building
  backend APIs.
cover: .gitbook/assets/CleanShot 2025-08-08 at 04.10.09@2x.png
coverY: 0
---

# Introduction

## CFSummit 2025 2-Day Workshop

[Register](https://www.eventbrite.com/e/workshop-building-reactive-uis-with-cbwire-tickets-1426617624719?aff=oddtdtcreator) for our upcoming 2-day CBWIRE workshop at CFSummit!

<figure><img src=".gitbook/assets/logo (1).png" alt=""><figcaption></figcaption></figure>

## Your First Component

Download and install [CommandBox](https://www.ortussolutions.com/products/commandbox). From your CLI, start CommandBox by typing 'box'. Then run the following:

```
mkdir cbwire-playground --cd
install cbwire@4
install commandbox-boxlang
coldbox create app
server start cfengine=boxlang javaVersion=openjdk21_jdk
```

Let's insert a counter element into our Main layout using **wire( "Counter" )**_._

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./layouts/Main.bxm --->
<bx:output>
<!doctype html>
<html>
    <body>
        <!--- Insert a counter here --->
        #wire( "Counter" )#
    </body>
</html>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./layouts/Main.cfm --->
<cfoutput>
<!doctype html>
<html>
    <body>
        <!--- Insert a counter here --->
        #wire( "Counter" )#
    </body>
</html>
</cfoutput>
```
{% endtab %}
{% endtabs %}

Let's define our Counter [component](the-essentials/components.md).&#x20;

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Counter.bx
class extends="cbwire.models.Component" {
    // Data properties
    data = {
        "counter": 0 // default value
    };

    // Action
    function increment() {
        data.counter++;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```python
// ./wires/Counter.cfc
component extends="cbwire.models.Component" {
    // Data properties
    data = {
        "counter": 0 // default value
    };

    // Action
    function increment() {
        data.counter++;
    }
}
```
{% endtab %}
{% endtabs %}

Finally, define our counter [template](the-essentials/templates.md).

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/counter.bxm --->
<bx:output>
    <div>
        <div>Count: #counter#</div>
        <button wire:click="increment">+</button>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/counter.cfm --->
<cfoutput>
    <div>
        <div>Count: #counter#</div>
        <button wire:click="increment">+</button>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

You now have a reactive counter that increments when you click the plus button without any page refreshing or writing JavaScript<mark style="color:blue;">!</mark> 🤯

![](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FEWvXvmxEDxwnAJzKYkxz%2Fuploads%2FPESERLoILEJOeO4yXCCn%2F2022-02-08%2016.02.25.gif?alt=media\&token=7846eb9)

## What!? How?

1. CBWIRE renders the component with default values (starting at 0).
2. A button click triggers Livewire.js to send a request to the server.
3. CBWIRE processes the request, runs `increment()`, and updates the data.
4. The updated HTML is sent back in the response.
5. Livewire.js refreshes the display using DOM comparison.

## Awesome, right?

* We developed a responsive counter.
* We avoided writing any JavaScript code.
* We didn't need to create an API.
* There was no need for page refreshes.
* We skipped using webpack or dealing with JavaScript compilation.
* We stayed entirely within our BoxLang / CFML environment. :nerd:

## Better With Alpine

The example shows how easily components interact with your back-end application. But for quick UI updates, JavaScript can handle changes without unnecessary server requests. While we don’t need a request to increment the counter, we might if we want to save it. This is where CBWIRE and Alpine work perfectly together.

[Alpine.js](https://alpinejs.dev/) is a lightweight JavaScript framework that was built for simplicity and designed to work alongside Livewire ( and therefore CBWIRE ).

Below, let's change our component to use Alpine.js and add a method to save the counter.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Counter.bx
class extends="cbwire.models.Component" {   
    data = {
        "counter": 0
    };

    function onMount() {
        // Load the counter from the session
        data.counter = session.counter ?: 0;
    }
    
    function save( counter ) {
        // Save the counter to the session
        session.counter = arguments.counter;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Counter.cfc
component extends="cbwire.models.Component" {   
    data = {
        "counter": 0
    };

    function onMount() {
        // Load the counter from the session
        data.counter = session.counter ?: 0;
    }
    
    function save( counter ) {
        // Save the counter to the session
        session.counter = arguments.counter;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/counter.bxm --->
<bx:output>
    <div
        x-data="{
            counter: $wire.counter,
            increment() {
                this.counter++   
            },
            async save() {
                // Call the save method on our component
                await $wire.save( this.counter );
            }
        }"
        wire:ignore.self>
        <div>Count: <span x-text="counter"></span></div>
        <button @click="increment">+</button>
        <button @click="save">Save</button>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/counter.cfm --->
<cfoutput>
    <div
        x-data="{
            counter: $wire.counter,
            increment() {
                this.counter++   
            },
            async save() {
                // Call the save method on our component
                await $wire.save( this.counter );
            }
        }"
        wire:ignore.self>
        <div>Count: <span x-text="counter"></span></div>
        <button @click="increment">+</button>
        <button @click="save">Save</button>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

The counter updates instantly on the client side, sending a server request only when clicking "Save" to store the value in the session. On page load, `onMount()` sets the counter from the session. Using `$wire`, Alpine communicates with the component, giving full control over server requests. CBWIRE is redefining web development—we think you'll love it!

## Credits

CBWIRE uses the JavaScript from [Livewire](https://laravel-livewire.com/) and [Alpine.js](https://alpinejs.dev/) for its DOM diffing and client-side functionality. CBWIRE wouldn't exist without the incredible work of [Caleb Porzio](https://x.com/calebporzio), the creator of both Livewire and Alpine. CBWIRE was created to bring these excellent tools into the ColdBox ecosystem.

The CBWIRE module for ColdBox is written and maintained by [Grant Copley](https://twitter.com/grantcopley), [Luis Majano](https://twitter.com/lmajano), and [Ortus Solutions](https://www.ortussolutions.com/).

## Project Support

Please consider becoming one of our lovingly esteemed [Patreon supporters](https://www.patreon.com/ortussolutions).
