# Actions

## Overview

Actions are methods on your [component](components.md) that either change the component's [data properties](properties.md) or perform some routine, such as updating your database or anything you can dream up in CFML.

Here is a basic example of how to use it:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Time.bx
class extends="cbwire.models.Component" {
    data = {
        "currentTime" : now()
    };
    function updateTime() {
        data.currentTime = now();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Time.cfc
component extends="cbwire.models.Component" {
    data = {
        "currentTime" : now()
    };
    function updateTime() {
        data.currentTime = now();
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- wires/time.bxm --->
<bx:output>
    <div>
        <p>Current time: #currentTime#</p>
        <button wire:click="updateTime">Update</button>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- wires/time.cfm --->
<cfoutput>
    <div>
        <p>Current time: #currentTime#</p>
        <button wire:click="updateTime">Update</button>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Actions do not need to return any value. Return values are ignored.
{% endhint %}

## Executing Actions

Livewire listens for browser events and invokes actions on your [component](components.md) using directives. These directives are used in your HTML [templates](templates.md) and follow the format: **wire:\[browser event]=\[action]**.

Some examples of events you can listen for include:

| Event   | Directive    |
| ------- | ------------ |
| click   | wire:click   |
| keydown | wire:keydown |
| submit  | wire:submit  |

```html
<button wire:click="doSomething">Do Something</button>

<input wire:keydown.enter="doSomething">

<form wire:submit="save">
    <button type="submit">Save</button>
</form>
```

{% hint style="info" %}
You can listen for any browser events on elements using the **wire:\[event]** directive, where \[event] is the event's name. For example, to listen for a "foo" event on a button element, you would use the following code:

```html
<button wire:foo="someAction">
```
{% endhint %}

On some elements, such as forms or links, you need to add a **.prevent** modifier to prevent the browser's default behavior. Otherwise, the browser will cause the page to reload and you will get unintended results.

```html
<a href="##" wire:click.prevent="doSomething">Do Something</a>
```

## Passing Parameters

You can pass parameters to actions such as **actionName( arg1, arg2, arg3 )**.

```html
<div>
    <button wire:click="addTask('Some Task')">Add Task</button>
</div>
```

The parameter is then passed through to your actions via function arguments.

```javascript
function addTask( taskName ){
    queryExecute( "
        insert into tasks (
            name
        ) values (
            :name
        )
    ", { name = arguments.taskName } ); // Adds 'Some Task'
}
```

## Magic Actions

There are a few magic actions already created on your [components](components.md).

| Action                        | Description                                            |
| ----------------------------- | ------------------------------------------------------ |
| $refresh                      | Will re-render the component without firing any action |
| $set( 'dataproperty', value ) | Shortcut to update the value of a property             |
| $toggle( 'dataproperty' )     | Shortcut to toggle boolean properties off and on       |

Consider the example below.

```javascript
data = {
    "message": ""
};

function setMessageToHello() {
    data.message = "Hello";
}
```

```html
<div>
    #message#
    <button wire:click="$set( 'message', 'Hello' )">Say Hi</button>
</div>
```
