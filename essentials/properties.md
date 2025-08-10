---
description: The state of the our components.
---

# Data Properties

Components have state known as Data Properties.

Data Properties are defined by creating a **data** structure on your component and assigning each property a default value.

```html
<!--- ./wires/SomeComponent.cfm --->
<cfscript>
    data = {
        "propertyName": "defaultValue"
    };
</cfscript>
```

{% hint style="info" %}
**Property names must be surrounded by single or double quotes.**

JavaScript is a case-sensitive language and CFML isn't. To preserve the casing of your property names, you must surround them with quotes.

```
// Bad
data = {
    propertyName: "defaultValue"
};

// Good
data = {
    "propertyName": "defaultValue"
};
```
{% endhint %}

{% hint style="info" %}
Data Properties are parsed by JavaScript and therefore can only store values that are JavaScript friendly: **strings, numeric, arrays, structs, or booleans**.
{% endhint %}

{% hint style="danger" %}
Data Properties are visible to JavaScript. **You SHOULD NOT store sensitive data in them**.
{% endhint %}

## Initial Values

Data properties should be assigned a default value, even if it's just an empty string.

```html
<cfscript>
    data = {
        "success": false,
        "message: "Hello World"
    };
</cfscript>
```

## Using Data Properties

You can access Data Properties from within your component [Actions](actions.md) like so.

```html
<cfscript>
    // Data properties
    data = {
        "task": ""
    };
    
    // Action
    function addTask(){
        taskService.create( data.task );
        data.task = "";
    }
</cfscript>
```

You can also reference Data Properties within your component template using **#propertyName#**_._

```html
<cfoutput>
    <div>
        <h1>Task</h1>
        <div>#task#</div>
    </div>
</cfoutput>
```

## Resetting Data Properties

You can reset properties back to their original value inside your [Actions](actions.md) using **reset()**.

<pre class="language-xml"><code class="lang-xml"><strong>&#x3C;cfscript>
</strong><strong>    // Data properties
</strong><strong>    data = {
</strong><strong>        "message" : "Hello world"
</strong><strong>    };
</strong><strong>    
</strong><strong>    // Actions    
</strong><strong>    function resetForm() {
</strong><strong>        reset();
</strong><strong>    }
</strong><strong>&#x3C;/cfscript>
</strong><strong>
</strong><strong>&#x3C;cfoutput>
</strong><strong>    &#x3C;div>
</strong><strong>        &#x3C;button wire:click="resetForm">Reset&#x3C;/button>
</strong><strong>    &#x3C;/div>
</strong><strong>&#x3C;/cfoutput>
</strong></code></pre>

You can reset individual or all Data Properties.

```javascript
function resetForm() {
    reset( "message" ); // resets 'message' data property
    reset( [ "message", "anotherprop" ] ); // reset multiple properties at once
    reset(); // resets all properties
}
```

You also can reset all properties EXCEPT the properties you pass.

```javascript
function resetForm() {
    resetExecept( "email" );
    resetExcept( [ "email", "phoneNumber" ] );
}
```

## Data Binding

You can bind your data properties and HTML elements using **wire:model**.

```html
<cfscript>
    // Data properties
    data = {
        "message" : "",
        "isChecked": false,
        "isSelected": "",
        "hero": ""
    };
</cfscript>

<cfoutput>
    <div>
        <form>
            <input wire:model="message" type="text">
            <input wire:model="isChecked" type="checkbox" value="true">
            <input wire:model="isSelected" name="isSelected" type="radio" value="true">
            <input wire:model="isSelected" name="isSelected" type="radio" value="false">
            <select wire:model="hero">
               <option value=""></option>
                <option value="Batman">Batman</option>
                <option value="Superman">Superman</option>
            </select>
        </form>
    </div>
</cfoutput>
```

There are also several [modifiers](../template-features/directives.md) you can add to control when and how requests are sent to your server.
