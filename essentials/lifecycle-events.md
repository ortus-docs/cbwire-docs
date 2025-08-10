---
description: Everything that has a beginning has an end. - The Oracle, Matrix
---

# Wire Lifecycle

## Order of Operations

When a [Wire](creating-components.md) is initially loaded (before any background AJAX requests are performed), the Lifecycle Methods are executed in the following order.

1. onMount()
2. Render [Computed Properties](computed-properties/)
3. onRender() or implicit [Template](templates.md) rendering

For background AJAX requests, lifecycle methods are executed in this order.

1. onHydrate\[DataProperty]\()
2. onHydrate()
3. Render [Computed Properties](computed-properties/)
4. Execute [Actions](actions.md)
5. onRender() or implicit [Template](templates.md) rendering

## onMount

Runs only **once** when the Wire is initially wired.&#x20;

{% hint style="warning" %}
This does not fire during subsequent renderings for the Wire.
{% endhint %}

```javascript
component extends="cbwire.models.Component" {

    data = {
        "taskId": 0
    };

    function onMount( event, rc, prc, parameters ){
        data.taskId = event.getValue( "taskId" );
    }

}

```

{% hint style="warning" %}
onMount() replaces what was formerly mount(). The mount() method is deprecated and will be removed in future major releases of CBWIRE.
{% endhint %}

## onHydrate

Runs on subsequent requests, after the Wire is hydrated, but before [Computed Properties](computed-properties/) are rendered, before an [Action](actions.md) is performed, or before onRender() is called.

```javascript
component extends="cbwire.models.Component" {

    data = {
        "hydrated": false
    };

    function clicked() {}

    function onHydrate( data ) {
        // Note that computed properties are not rendered yet
        data.hydrated = true;
    }

    function onRender( args ) {
        return "
            <div>
                <div>Hydrated: #args.hydrated#</div>
                <button wire:click='clicked'>Click me</button>
            </div>
        ";
    }
}
```

## onHydrate\[ DataProperty ]

Runs on subsequent requests, after a specific [Data Property](properties.md) is hydrated, but before [Computed Properties](computed-properties/) are rendered, before an [Action](actions.md) is performed, or before onRender() is called.

```javascript
component extends="cbwire.models.Component" {

    data = {
        "count": 1
    };

    function onHydrateCount( data ) {
        // Note that computed properties are not rendered yet
        data.count += 1;
    }  
}
```

## onRender

Runs during the rendering of a [Wire](creating-components.md). If onRender() is defined on the Wire, CBWIRE will use what is returned as the component's template, otherwise it will perform a lookup for a view template. The `args` parameter is provided which contains both the [Data Properties](properties.md) and any rendered [Computed Properties](computed-properties/).

```javascript
component extends="cbwire.models.Component" {

    data = {
        "rendered": true
    };
    
    computed = {
        "currentDate": function() {
            return now();
        }
    }

    function onRender( args ) {
        return "
            <div>
                <div>Rendered: #args.rendered# at #args.currentDate#</div>
            </div>
        ";
    }
}
```
