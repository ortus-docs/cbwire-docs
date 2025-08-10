# Data Properties

## Overview

Data Properties hold the state of our component and are defined with a **data** structure in your [component](components.md). Each data property is assigned a default value.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
//./wires/SomeComponent.bx
class extends="cbwire.models.Component" {
    data = {
        "propertyName": "defaultValue"
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
//./wires/SomeComponent.cfc
component extends="cbwire.models.Component" {
    data = {
        "propertyName": "defaultValue"
    };
}
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
JavaScript parses your data properties. Your data properties can only store JavaScript-friendly values: strings, numerics, arrays, structs, or booleans.
{% endhint %}

{% hint style="warning" %}
**Single or double quotes must surround property names.**

JavaScript is a case-sensitive language, and CFML isn't. To preserve the casing of your property names, you must surround them with quotes.

Don't do this.

```javascript
data = {
    propertyName: "defaultValue"
};
```
{% endhint %}

{% hint style="danger" %}
Data properties are visible to JavaScript. You SHOULD NOT store sensitive data in them.
{% endhint %}

## Accessing From Actions

Using the **data** structure, you can access data properties from within your component [actions](actions.md).

```javascript
// Data properties
data = {
    "time": now()
}; 
   
// Action
function updateTime(){
    data.time = now();
}
```

## Accessing From Templates

You can access data properties within your [templates](templates.md) using **#propertyName#**_._

{% tabs %}
{% tab title="Boxlang" %}
```html
<bx:output>
    <div>
        <h1>Current time</h1>
        <div>#time#</div>
        <button wire:click="updateTime">Update</button>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<cfoutput>
    <div>
        <h1>Current time</h1>
        <div>#time#</div>
        <button wire:click="updateTime">Update</button>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Resetting Properties

You can reset all data properties to their original default value inside your [actions](actions.md) using **reset()**.

```javascript
data = {
    "time": now()
};

function resetTime(){
    reset();
}
```

You can reset individual, some, or all data properties.

```javascript
function resetForm() {
    reset( "message" ); // resets 'message' data property
    reset( [ "message", "anotherprop" ] ); // reset multiple properties at once
    reset(); // resets all properties
}
```

You can also reset all properties EXCEPT the properties you pass.

```javascript
function resetForm() {
    resetExecept( "email" );
    resetExcept( [ "email", "phoneNumber" ] );
}
```
