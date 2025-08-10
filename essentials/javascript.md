---
description: >-
  Call Actions, emit and listen for Events, and more - all from Vanilla
  JavaScript.
---

# JavaScript

CBWIRE provides a handful of JavaScript hooks and functionality you can use as needed.

## Inline JavaScript

We recommend you use [AlpineJS](../integrations/alpinejs.md) for most of your JavaScript needs, but you can use _\<script>_ tags directly inside your components.

```html
<div>
    <!--- Your component's template --->
    <script>
        document.addEventListener('livewire:load', function () {
            // Your JS here.
        })
    </script>
</div>
```

{% hint style="info" %}
Your scripts will be run only once upon the first render of the component. If you need to run a JavaScript function later, you can emit the [Event](events.md) from the component and listen to it in JavaScript.
{% endhint %}

## Lifecycle Hooks

CBWIRE allows you to execute JavaScript during various events.

<table><thead><tr><th width="251">Hook</th><th>Description</th></tr></thead><tbody><tr><td>component.initialized</td><td>Called when a component has been initialized on the page by CBWIRE</td></tr><tr><td>element.initialized</td><td>Called when CBWIRE initializes an individual element</td></tr><tr><td>element.updating</td><td>Called before CBWIRE updates an element during its DOM-diffing cycle after a network roundtrip</td></tr><tr><td>element.updated</td><td>Called after CBWIRE updates an element during its DOM-diffing cycle after a network roundtrip</td></tr><tr><td>element.removed</td><td>Called after CBWIRE removes an element during its DOM-diffing cycle</td></tr><tr><td>message.sent</td><td>Called when a CBWIRE update triggers a message sent to the server via AJAX</td></tr><tr><td>message.failed</td><td>Called if the message send fails for some reason</td></tr><tr><td>message.received</td><td>Called when a message has finished its roundtrip, but before CBWIRE updates the DOM</td></tr><tr><td>message.processed</td><td>Called after CBWIRE processes all side effects (including DOM-diffing) from a message</td></tr></tbody></table>

```html
<cfoutput>
    <div>
        <!-- HTML goes here -->
        <script>
            document.addEventListener("DOMContentLoaded", () => {
                cbwire.hook('component.initialized', (wire) => {})
                cbwire.hook('element.initialized', (el, wire) => {})
                cbwire.hook('element.updating', (fromEl, toEl, wire) => {})
                cbwire.hook('element.updated', (el, wire) => {})
                cbwire.hook('element.removed', (el, wire) => {})
                cbwire.hook('message.sent', (message, wire) => {})
                cbwire.hook('message.failed', (message, wire) => {})
                cbwire.hook('message.received', (message, wire) => {})
                cbwire.hook('message.processed', (message, wire) => {})
            });
        </script>
    </div>
</cfoutput>

<cfscript>
    <!--- Component definition goes here --->
</cfscript>
```

## Component Interaction

You can interact with your component's actions, set data properties, and more, using _cbwire.find_. Once you have a reference to the component, you can use the methods below.

```html
<cfoutput>
    <div>
        <!-- HTML goes here -->
        <script>
            document.addEventListener("livewire:load", function() {
                // _id contains the id of our wire
                var component = cbwire.find('#_id#');
                // gets the value of a data property called 'count'
                var count = component.count;
                // updates the values of a data property
                component.count = 5;
                // calls the increment action on our wire
                component.increment();          
                // calls the addTask action and passes parameters   
                component.addTask( 'someTask' ); 
                // same as increment call above
                component.call( 'increment' ); 
                // On someEvent, console log
                component.on( 'someEvent', function() {
                    console.log('Got someEvent');
                } );
                // emits someEvent and pass parameters
                component.emit('someEvent', 'foo', 'bar');
            } );
        </script>
    </div>
</cfoutput>
```
