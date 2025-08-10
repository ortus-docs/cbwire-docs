---
description: Create dynamic properties for your UI Wires.
---

# Computed Properties ( Proxied )

{% hint style="warning" %}
You must be on CBWIRE version 2.3.5 or later and enable the [Configuration](../configuration.md) setting **useComputedPropertiesProxy** to follow this guide.

Otherwise, please follow [this guide](./) instead.
{% endhint %}

{% hint style="success" %}
In CBWIRE 2.3.5, we introduced the ability to proxy Computed Properties, which offers several advantages, including:

* Computed Properties are passed around as closures and are only rendered when called.
* The ability to invoke Computed Properties anywhere they are needed (both Actions and Templates).
* The ability to cache Computed Property results to enhance performance.
{% endhint %}

Computed Properties are dynamic properties and help derive values from a database or another persistent store like a cache.&#x20;

Computed Properties are similar to [Data Properties](../properties.md) with some key differences:

* They are declared as inline functions using `computed`.
* They can return any CFML value or object.

## Defining Properties

Define Computed Properties in your [Wires](../creating-components.md) using `computed`.&#x20;

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

## Accessing From Actions

You can access your Computed Properties from within your [Actions](../actions.md) using `computed.[propertyName]()`.

```javascript
component extends="cbwire.models.Component" {

    // Computed Properties
    computed = {
        "allTasks": function() {
            // return something here
        }
    };

    // Action
    function deleteTasks() {      

        if ( arrayLen( computed.allTasks() ) {
            taskService.deleteAll();
        }

    }
}
```

## Accessing From Templates

You can access Computed Properties in your [Template](computed-properties-proxied.md#templates) using `args.computed.[computedProperty]()`.

```html
<cfoutput>
<div>
    <ul>
        <cfloop array="#args.computed.allTasks()#" index="task">
            <li>#task#</li>    
        </cfloop>
    </ul>
</div>
</cfoutput>
```
