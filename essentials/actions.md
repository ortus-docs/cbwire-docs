---
description: Methods that are invoked based on user interactions.
---

# Actions

Actions are methods on your component that either change the component's state ( Data Properties ) or perform some action needed ( such as updating your database ).

Here is a basic example of how to use it:

```html
<cfscript>
    data = {
        "currentTime" : now()
    };
    
    function updateTime() {
        data.currentTime = now();
    }
</cfscript>

<cfoutput>
    <p>
        Current time: #currentTime#
    </p>
    <button wire:click="updateTime">Update</button>
</cfoutput>
```

{% hint style="info" %}
Actions do not need to return any value. Return values are ignored.
{% endhint %}

## Running Actions

CBWIRE listens for browser events and invokes actions using [Directives](../template-features/directives.md). The directives follow the format: **wire:\[browser event]=\[action]**.

Some examples of events you can listen for include:

| Event   | Directive    |
| ------- | ------------ |
| click   | wire:click   |
| keydown | wire:keydown |
| submit  | wire:submit  |

Here are a few examples of each in HTML:

```html
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

On some elements such as forms or links, you need to add a **.prevent** modifier to prevent the browser's default behavior. Otherwise, the browser will cause a page reload and you will get unintended results.

```html
<a href="" wire:click.prevent="doSomething">Do Something</a>
```

## Passing Parameters

You can pass parameters to your actions, such as here using **actionName( params )**.

```html
<cfoutput>
    <div>
        <button wire:click="addTask('Some Task')">Add Task</button>
    </div>
</cfoutput>
```

The parameter is then passed through to your [Actions](actions.md) via function arguments.

```javascript
// Action
function addTask( taskName ){
    queryExecute( "
        insert into tasks (
            name
        ) values (
            :name
        )
    ", { name = arguments.taskName } );
}
```

## Magic Actions

There are a few magic actions already available to your components.

| Action                        | Description                                            |
| ----------------------------- | ------------------------------------------------------ |
| $refresh                      | Will re-render the component without firing any action |
| $set( 'dataproperty', value ) | Shortcut to update the value of a property             |
| $toggle( 'dataproperty' )     | Shortcut to toggle boolean properties off and on       |

Consider the example below.

```html
<cfscript>
    data = {
        "message": ""
    };
    
    function setMessageToHello() {
        data.message = "Hello";
    }
</cfscript>

<div>
    #message#
    <button wire:click="$set( 'message', 'Hello' )">Say Hi</button>
</div>
```

## Refresh

Sometimes you may need to force your component to completely refresh in the DOM. This is particularly true when you have nested components.

```html
<cfoutput>
    <div>
        <cfif showTeamForm>
            #wire( "TeamForm" )#
        <cfelse>
            <button wire:click="selectTeamForm">Show Team Form</button>
            #wire( "UserForm" )#
        </cfif>
    </div>
</cfoutput>
```

To force a refresh, simply call **refresh()** from any action on your component.

```javascript
<cfscript>
    data = {
        "showTeamForm": false
    };

    function selectTeamForm() {
        data.showTeamForm = true;      
        refresh(); // force a refresh
    }
</cfscript>
```
