---
description: >-
  The magic sauce. Actions allow you to easily listen to page interactions and
  call a method on your Wires.
---

# Actions

Actions provide an effortless way to track page interactions and invoke methods on your [Wires](creating-components.md), resulting in a re-render of the Wire's [Template](templates.md).

Here is a basic example of how to use it:

```javascript
component extends="cbwire.model.Component"{
  
    // Actions
    function addTask(){
      queryExecute( ... );
    }
}
```

```html
<!--- Template --->
<div>
    <button wire:click="addTask">Add Task</button>
</div>
```

## Invoking Actions

You can listen for browser events and invoke actions using a selection of various [Directives](../template-features/directives.md). The directives follow the format: **wire:\[browser event]=\[action]**.

Some examples of events you can listen for include:

| Event   | Directive    |
| ------- | ------------ |
| click   | wire:click   |
| keydown | wire:keydown |
| submit  | wire:submit  |

Here are a few examples of each in HTML:

```html
<a href="" wire:click.prevent="doSomething">Do Something</a>

<button wire:click="doSomething">Do Something</button>

<input wire:keydown.enter="doSomething">

<form wire:submit.prevent="save">
    <button>Save</button>
</form>
```

You can listen for any browser events on elements by using the **wire:\[event]** directive, where \[event] is the event's name. For example, to listen for a "foo" event on a button element, you would use the following code:&#x20;

```html
<button wire:foo="someAction">
```

## Passing Parameters

You can pass parameters to your actions, such as here using `addTask('Some Task')`.

```html
<button wire:click="addTask('Some Task')">Add Task</button>
```

The parameter is then passed through to your [Actions](actions.md) via function arguments.

```javascript
// Action
function addTask( taskName ){

}
```

## Return Values

{% hint style="warning" %}
Actions shouldn't return any value. Return values are ignored.
{% endhint %}

## Accessing Data Properties

You can access data properties inside your actions directly using `data`.

```javascript
    data = {
        "task": ""
    };

    // Actions
    function clearTasks(){
        data.task = "";
    }
```

## Accessing Computed Properties

You can access Computed Properties inside your actions directly using `computed`.

```javascript
    // Computed Properties
    computed = {
        "tasks": function() {
            return queryExecute( "
                select *
                from tasks
            " );

        }
    };

    // Actions
    function deleteTasks(){
        if ( computed.tasks.recordCount ) {
            // query to delete the tasks...    
        } 
    }
```

{% hint style="info" %}
Notice that our Computed Property above is defined as a closure function, but once our deleteTasks() action is called, the computed property has already been rendered.
{% endhint %}

## Magic Actions

In CBWIRE, there are some "magic" actions that are usually prefixed with a "$" symbol:

| Function                      | Description                                            |
| ----------------------------- | ------------------------------------------------------ |
| $refresh                      | Will re-render the component without firing any action |
| $set( 'dataproperty', value ) | Shortcut to update the value of a property             |
| $toggle( 'dataproperty' )     | Shortcut to toggle boolean properties off and on       |

Consider the example below.

```html
<div>
    #args.message#
    <button wire:click="setMessageToHello">Say Hi</button>
</div>
```

You can instead call $set and avoid the need to create an Action named setMessageToHello.

```html
<div>
    #args.message#
    <button wire:click="$set( 'message', 'Hello' )">Say Hi</button>
</div>
```

It can also be used in the backend when listening for an event. For example, if you have one component that emits an event like this:

```javascript
function someAction() {
    emit( "some-event" );
}
```

Then in another component you can use a magic action for example $refresh() instead of having to point the listener to a method:

```javascript
component {
    listeners = {
        "some-event": "$refresh"
    };
}
```
