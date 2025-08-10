---
description: You can emit events from both your Wires and JavaScript. Superb!
---

# Events & Listeners

## Emitting Events

You can emit events from [Actions](actions.md), [Templates](templates.md), and JavaScript.

### Actions

Using the `emit` method:

```javascript
function someAction() {
    emit( "taskAdded" );
}
```

You can provide arguments to all listeners by passing an array as the second argument.

```javascript
function someAction() {
    emit( "taskAdded", [
        "some task",
        now()
    ] );
}
```

{% hint style="info" %}
When emitting events, arguments must be passed as an array to ensure proper positioning both within CFML and JavaScript.
{% endhint %}

### Templates

Using the `$emit()` method:

```markup
<button wire:click="$emit( 'taskAdded' )">
```

### JavaScript

Using the global `cbwire.emit()`:

```xml
<script>
    cbwire.emit( 'taskAdded' );
</script>
```

## Listeners

You can register event listeners on a Wire by defining `listeners`.

```javascript
component extends="cbwire.models.Component"{
    
    listeners = {
        "taskAdded": "clearCache"
    };

    function clearCache(){}
}
```

{% hint style="info" %}
The `clearCache` method will be invoked when a `taskAdded` event is emitted.
{% endhint %}

{% hint style="warning" %}
JavaScript keys are case-sensitive. You can preserve the key casing in CFML by surrounding your listener names in quotations.
{% endhint %}

### JavaScript

Listening to events that you emit in your [Actions](events.md#actions) can be done using `cbwire.on()`.

```html
<script>
    cbwire.on( 'taskAdded', task => {
        console.log( 'A task was added. ');
    })
</script>
```
