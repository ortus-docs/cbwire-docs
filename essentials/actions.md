# Actions

Actions in cbwire are methods in your cbwire component that are invoked when user interactions occur. Once an action has been invoked, the component's template is re-rendered.

```javascript
// File: ./wires/Counter.cfc
component extends="cbwire.model.Component"{

    // Data property
    variables.data = {
        "counter": 0
    };

    // Action
    function increment(){
        variables.data.counter += 1;
    }
}
```

```xml
<div>
    <button wire:click="increment">Increment Counter</button>
</div>
```
