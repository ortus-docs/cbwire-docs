---
description: >-
  Wires are reactive UI elements you create. They include a powerful feature-set
  provided by CBWIRE.
---

# Wires

Wires primarily consist of the following:

* [Data Properties](properties.md)\
  Reactive properties that we can easily change.
* [Computed Properties](computed-properties/)\
  Properties that are computed every time our template is rendered.
* [Actions](actions.md)\
  Methods that make state changes to our data properties.
* [Events and Listeners](events.md)\
  Listen for emitted events within Wires or client-side JavaScript.
* [Templates](templates.md)\
  HTML to render our Wire.

## Basic Example

```javascript
// File: wires/TaskList.cfc
component extends="cbwire.models.Component" {

    // Data properties
    data = {
        "task": "",
        "tasks": []
    };
    
    // Computed properties
    computed = {
        "counter": function() {
            return arrayLen( data.tasks );
        }
    };
    
    // Listeners
    listeners = {
        "taskAdded": "sendEmail"
    };
    
    // Action
    function addTask(){
        data.tasks.append( data.task );
        this.emit( "taskAdded", data.task );
    }
    
}
```

```html
<!--- /views/wires/tasklist.cfm --->
<cfoutput>
<div>
    <div>Number of tasks: #args.computed.counter()#</div>
    <div><input wire:model="task"></div>
    <div><button wire:click="addTask">Add Task</button></div>
</div>
</cfoutput>
```

## Scaffolding

You can scaffold Wires quickly using the [commandbox-cbwire](https://github.com/commandbox-modules/commandbox-cbwire) module.

From our CommandBox shell, type:

```
install commandbox-cbwire
```

Let's create a **Counter** Wire that we will use to list our favorite movies.

```javascript
cbwire create Counter
// Created /wires/Counter.cfc
// Created /views/wires/counter.cfm
// Created /tests/specs/integration/wires/CounterTest.cfc
```

You can provide named arguments as well.

```javascript
cbwire create name=Counter views=false
// Created /wires/Counter.cfc
// Created /tests/specs/integration/wires/CounterTest.cfc
```
