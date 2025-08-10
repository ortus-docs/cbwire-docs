---
description: >-
  Seamlessly connect between the front-end and your server back-end using
  JavaScript and CFML.
---

# JavaScript

## Lifecycle Hooks

CBWIRE gives you the opportunity to execute JavaScript during various events.

<table><thead><tr><th width="251">Hook</th><th>Description</th></tr></thead><tbody><tr><td>component.initialized</td><td>Called when a <a href="creating-components.md">Wire</a> has been initialized on the page by Livewire</td></tr><tr><td>element.initialized</td><td>Called when Livewire initializes an individual element</td></tr><tr><td>element.updating</td><td>Called before Livewire updates an element during its DOM-diffing cycle after a network roundtrip</td></tr><tr><td>element.updated</td><td>Called after Livewire updates an element during its DOM-diffing cycle after a network roundtrip</td></tr><tr><td>element.removed</td><td>Called after Livewire removes an element during its DOM-diffing cycle</td></tr><tr><td>message.sent</td><td>Called when a Livewire update triggers a message sent to the server via AJAX</td></tr><tr><td>message.failed</td><td>Called if the message send fails for some reason</td></tr><tr><td>message.received</td><td>Called when a message has finished its roudtrip, but before Livewire updates the DOM</td></tr><tr><td>message.processed</td><td>Called after Livewire processes all side effects (including DOM-diffing) from a message</td></tr></tbody></table>

```html
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
```

## Interacting With Wires

You can interact with your [Wires](creating-components.md), calling [Actions](actions.md), setting [Data Properties](properties.md), and more, using **cbwire.find** from within your [Wire Template](templates.md). Once you have a reference to the Wire, you can interact with it using the methods below.

```html
<script>
    document.addEventListener("livewire:load", function() {
        var thisWire = cbwire.find('#args._id#'); // args._id contains the id of our wire
    
        var count = thisWire.count; // gets the value of a data property called 'count'   
        
        thisWire.count = 5; // updates the values of a data property
    
        thisWire.increment(); // calls the increment action on our wire
        
        thisWire.addTask( 'someTask' ); // calls the addTask action and passes parameters 
        
        thisWire.call( 'increment' ); // same as increment call above 
    
        // On someEvent, console log
        thisWire.on( 'someEvent', function() {
            console.log('Got someEvent');
        } );  
        
        thisWire.emit('someEvent', 'foo', 'bar'); // emits someEvent and pass parameters
    } );
</script>
```
