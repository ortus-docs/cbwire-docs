---
description: Create dynamic properties for your UI Wires.
---

# Computed Properties

{% hint style="warning" %}
If you are on CBWIRE version 2.3.x  and have enabled the configuration property **useComputedPropertiesProxy**, then you will need to follow [this guide here](computed-properties-proxied.md) instead as this fundamentally changes how computed properties are referenced and accessed.&#x20;
{% endhint %}

Computed Properties are dynamic properties and are helpful for deriving values from a database or another persistent store like a cache.&#x20;

Computed Properties are similar to [Data Properties](../properties.md) with some key differences:

* They are declared as inline functions using `computed`.
* They can return any type of CFML value or object, not just values that can be serialized and parsed by JavaScript like Data Properties.

{% hint style="success" %}
See the [Wire Lifecycle](../lifecycle-events.md#order-of-operations) page for details on when Computed Properties are executed.
{% endhint %}

{% hint style="info" %}
Computed Properties are cached for the lifetime of the request. Meaning, if you reference your Computed Property three times in your [Template](./#templates) or from within a wire [Action](../actions.md), it will only execute once.
{% endhint %}

## Defining Properties

You can define Computed Properties on your [Wires](../creating-components.md) using `computed`. Each Computed Property is invoked only once during a single request cycle.

```javascript
component extends="cbwire.models.Component" {

    property name="taskService" inject="taskService@myapp";

    // Computed Properties
    computed = {
        "allTasks": function() {
            return taskService.getAll();
        }
    };
}

```

## Accessing Properties

You can access your Computed Properties from within your [Actions](../actions.md) using `computed.[propertyName]`.

```javascript
component extends="cbwire.models.Component" {

    property name="taskService" inject="taskService@myapp";

    // Computed Properties
    computed = {
        "allTasks": function() {
            return taskService.getAll();
        }
    };

    // Action
    function deleteTasks() {
        
        // Notice below we don't invoke the method using computed.allTasks().
        // We instead just use 'computed.allTasks'.
        // At this point, cbwire has rendered our computed properties
        // and overwritten the references with their result.
        if ( arrayLen( computed.allTasks ) {
            taskService.deleteAll();
        }

    }
}
```

### Getters

Getters are automatically created based on the property's name.&#x20;

In the example below, you to access the property using `this.getAllTasks()`. You can use this as an alternative to using `computed`.

```javascript
component extends="cbwire.models.Component" {

    property name="taskService" inject="taskService@myapp";

    // Computed Properties
    computed = {
        "allTasks": function() {
            return taskService.getAll();
        }
    };

    // Action
    function deleteTasks() {
        if ( arrayLen( this.getAllTasks() ) {
            taskService.deleteAll();
        }
    }
}
```

### Templates

You can access Computed Properties in your [Template](./#templates) via the `args` scope.

```html
<cfoutput>
<div>
    <ul>
        <cfloop array="#args.allTasks#" index="task">
            <li>#task#</li>    
        </cfloop>
    </ul>
</div>
</cfoutput>
```
