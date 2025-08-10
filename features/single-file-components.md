# Single-file Components

## Overview

[Components](../the-essentials/components.md) are typically built with a **.bx and a .bxm file (Boxlang )** or a **.cfc file and a .cfm file (CFML)**. You can combine these into a single .bxm or .cfm file, similar to how components in Vue.js are built.

Let's look at the example from the [introduction](../) that uses two separate files.

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

```html
<!--- ./wires/counter.bxm|cfm --->
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
```

## Single File Format

Let's combine these into a single file.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/counter.bxm --->
<bx:script>
    // @startWire
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
    // @endWire
</bx:script>

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
<cfscript>
    // @startWire
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
    // @endWire
</cfscript>

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

There are only a few differences with the single-file format.

* The contents of your **.cfc** file are instead placed inside a **\<cfscript>** block
* The **// @startWire** and **// @endWire** comment markers have been added so that CBWIRE can parse the file.

{% hint style="warning" %}
You must add the **//@startWire** and **//@endWire** markers when using single-file format.
{% endhint %}

{% hint style="info" %}
You can place your **\<cfscript>** block above or below your template. CBWIRE parses this out before processing.
{% endhint %}

## Performance

There is a slight performance hit when using the single file format because CBWIRE has to parse out the sections of your file. However, this hit is minimal because of the internal caching CBWIRE uses.

{% hint style="info" %}
Cached files can be found under **./modules/cbwire/models/tmp**. This directory gets cleared anytime you reinit your application using ColdBox's **fwreinit**.

http://myapp/?fwreinit=true
{% endhint %}
