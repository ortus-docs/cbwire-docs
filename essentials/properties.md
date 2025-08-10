---
description: >-
  Similar to Vue.js and other frontend JavaScript frameworks, you can define and
  initialize reactive properties on your cbwire components.
---

# Data Properties

## Initializing Properties

You can define and initialize reactive properties on your cbwire components using `variables.data`.&#x20;

{% hint style="success" %}
When data properties are mutated, the UI will update also.
{% endhint %}

```javascript
// File: wires/Counter.cfc
component extends="cbwire.models.Component"{
    variables.data = {
        "counter": 0
    };
    ...
}
```

## Referencing Properties

You can reference a property directly in your component using `variables.data.[ property name ]`.

```javascript
// File: wires/Counter.cfc
component extends="cbwire.models.Component"{

    // Data properties
    variables.data = {
        "counter": 0
    };
    
    // Action called from UI
    function increment(){
        variables.data.counter += 1;
    }
    ...
}
```

You can also reference any defined properties from your cbwire component view using `args.[ property name  ]`.

```xml
// File: views/wires/counter.cfm
<cfoutput>
<div>
    <h1>Counter</h1>
    <div>Count: #args.counter#</div>
</div>
</cfoutput>
```

## Things To Know

Your component's private `variables` scope will hold your data property definitions and current values, which is seemingly secure, but it's necessary to be cautious about what you store. cbwire communicates with the server via Livewire using AJAX requests and includes the current state of the data properties within those requests.

{% hint style="success" %}
cbwire includes the current values of the data properties during requests to determine what state has changed and if Livewire should update the [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction).
{% endhint %}

{% hint style="danger" %}
You should NEVER store sensitive data ( such as passwords, SSNs ) that you wouldn't want your application's users to see.
{% endhint %}

{% hint style="info" %}
Data properties can only be data types that are castable to JavaScript data types, such as CFML strings, numeric, arrays, structs, or booleans.
{% endhint %}

