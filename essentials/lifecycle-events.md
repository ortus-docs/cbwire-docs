---
description: Various component lifecycle events you can leverage for your needs.
---

# Lifecycle Methods

CBWIRE provides various lifecycle methods you can hook into to update and render your components.

## Order of Operations

The lifecycle methods are executed in the following order **when a component is initially loaded**:

1. onMount()
2. Render component

**For subsequent AJAX requests**, lifecycle methods are executed in this order.

1. onHydrate\[DataProperty]\()
2. onHydrate()
3. onUpdate\[DataProperty]\()
4. onUpdate()
5. Execute actions
6. Render component

## onMount

Runs only once when a component is initially wired. This can be used to inject data values into your component either via params you pass in when calling wire(), or pulling in values from the RC or PRC scopes.

```javascript
<cfscript>
    function onMount( event, rc, prc, params ){
        data.someProperty = rc.someValue;
        data.anotherProperty = params.passedInParam;
    }
</cfscript>
```

{% hint style="warning" %}
onMount() only fires when the component is initially rendered and does not fire on subsequent requests when your component re-renders. This can cause issues when referencing things such as the RC or PRC scope. If you are passing in values with the RC or PRC scope, you will need to store the values as data properties to ensure they are available to your component in subsequent requests.
{% endhint %}

## onHydrate

Runs on subsequent requests after a component is hydrated, but before computed properties are rendered, before a property is updated or action is performed, or before the component is rendered.

```javascript
<cfscript>
    function onHydrate( data ) {
        // Note that computed properties are not rendered yet
        data.hydrated = true;
    }
</cfscript>
```

## onHydrate\[ Property ]

Runs on subsequent requests, after a specific data property is hydrated, but before computed properties are rendered, before an action is performed, or before the component is rendered.

```javascript
<cfscript>
    data = {
        "count": 1
    };

    function onHydrateCount( data ) {
        // Note that computed properties are not rendered yet
        data.count += 1;
    }  
</cfscript>
```

## onUpdate\[ Property ]

Runs on subsequent requests, after a data property is updated using wire:model or $set. Only runs when the targeted data property is updated.

<pre class="language-javascript"><code class="lang-javascript"><strong>&#x3C;cfscript>
</strong>    data = {
        "count": 1
    };
    
    function onUpdateCount( newValue, oldValue ) {
        if ( newValue >= 100 ) {
            // Reset back to 1
            data.count = 1;
        }

    }
&#x3C;/cfscript>
</code></pre>

## onUpdate

Runs on subsequent requests, after any data property is updated using wire:model or $set.

{% hint style="info" %}
onUpdate() will only fire if the incoming request is updating a single data property, such as when using wire:model.&#x20;
{% endhint %}

```javascript
<cfscript>
    function onUpdate( newValues, oldValues ) {
        // ....
    }
</cfscript>
```

## onRender

Runs during the rendering of a component. If _onRender()_ is defined, CBWIRE will use what is returned as the component's template. Otherwise, it will render the component normally.&#x20;

```javascript
<cfscript>
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
</cfscript>
```
