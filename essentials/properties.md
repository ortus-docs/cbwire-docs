---
description: Define reactive properties for your UI Wires with just a few lines of code.
---

# Data Properties

Similar to Vue.js and other frontend JavaScript frameworks, you can define reactive properties on your [Wires](creating-components.md).

## Defining Properties

You can define and initialize properties on your [Wires](creating-components.md) using `data`.&#x20;

```javascript
component extends="cbwire.models.Component"{
    data = {
        "task": ""
    };
}
```

{% hint style="warning" %}
Data Properties are parsed and tracked by Livewire and JavaScript, therefore only data types that are castable to JavaScript can be stored in Data Properties (strings, numeric, arrays, structs, or booleans).\
\
If you are needing more complex data types for your templates, use [Computed Properties](computed-properties/) instead.
{% endhint %}

{% hint style="success" %}
When data properties are mutated, the UI will update also.
{% endhint %}

## Accessing Properties

### Data Variable

You can reference a property using `data.[propertyName]`.

```javascript
component extends="cbwire.models.Component"{

    property name="taskService" inject="TaskService@myapp";
    
    // Data properties
    data = {
        "task": ""
    };
    
    // Action
    function addTask(){
        taskService.create( data.task );
    }
}
```

### Getters

Getter methods are automatically available for your properties based on the property's name. You to access the property within your Wire using `this.get[propertyName]()`.&#x20;

```javascript
component extends="cbwire.models.Component"{
    
    property name="taskService" inject="TaskService@myapp";

    // Data properties
    data = {
        "task": ""
    };
    
    // Action
    function addTask(){
        taskService.create( this.getTask() );
    }
}
```

{% hint style="success" %}
You can use this as an alternative to using `data`.
{% endhint %}

### Templates

You can reference data properties within your Wire's template using `args.[propertyName]`.

```xml
<cfoutput>
<div>
    <h1>Task</h1>
    <div>#args.task#</div>
</div>
</cfoutput>
```

## Resetting Properties

You can reset properties back to their original value using `reset()`.

```javascript
component extends="cbwire.models.Component"{

    // Data properties
    data = {
        "task": "Some task here"
    };

    // Action
    function resetForm() {
        data.task = "Another value";
        reset( "task" ); // Reverts data.task to 'Some task here'
    }

}
```

There are several ways to use reset.

```javascript
function someAction() {
    reset( "task" ); // resets 'task' data property
    reset( [ "task", "anotherprop" ] ); // reset multiple properties at once
    reset(); // resets all properties
}
```

## Security

Your component's private `variables` scope will hold your data property definitions and current values, but it's necessary to be cautious about what you store.&#x20;

{% hint style="success" %}
The values of your data properties are included with each XHR response and Livewire uses these values to determine what should be updated in the [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction).
{% endhint %}

{% hint style="danger" %}
You should NEVER store sensitive data ( such as passwords, and SSNs ) that you wouldn't want your application's users to see.
{% endhint %}

