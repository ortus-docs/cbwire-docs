---
description: Front-end testing of your UI Wires using a beautiful test API.
---

# Testing

You can easily test your [Wires](creating-components.md) by extending `cbwire.models.BaseWireTest` and using our fluent testing API in your [TestBox](https://testbox.ortusbooks.com/) specs.

## Test Example

You can invoke your [Wires](creating-components.md) for testing by using `wire()`.&#x20;

```javascript
component extends="cbwire.models.BaseWireTest" {

    function run(){

        describe( "TaskList.cfc", function(){
	
	    it( "calling 'clearTasks' removes the tasks", function() {
        
                wire( "TaskList" )
                    // Sets our data properties
                    .data( "tasks", [ "task1" ] )
                    // Verifies output in our template rendering
                    .see( "<li>task 1</li>" )
                    // Runs the 'clearTasks' action
                    .call( "clearTasks" )
                    // Verifies updated template rendering
		    .dontSee( "<li>task 1</li>" );
		} );
	
	} );
    }
}
```

## Test Methods

### data

Set a [Data Property](properties.md) to the specified value.

```javascript
wire( "TaskList" )
    .data( "tasks", [ "task1", "task2" ] ) // one property at a time
    .data( { "tasks" : [ "task1", "task2" ] } ); // struct of properties
```

### computed

Set a [Computed Property](computed-properties/) to the specified closure.

```javascript
wire( "TaskList" )
    .computed( "count", function() { return 3; } ) // one property at a time
    .computed( { "count" : function() { return 3; } } ); // struct of properties
```

### toggle

Toggles a [Data Property](properties.md) between true and false.

```javascript
wire( "TaskList" ).toggle( "showModal" );
```

### call

Calls an [Action](actions.md). Optional array of parameters can be provided.

```javascript
wire( "TaskList" ).call( "clearTasks" );
wire( "TaskList" ).call( "clearTasks", [ param1, param2 ] );
```

### emit

Emits an [Event](events.md) and fires any [Listeners](events.md#listeners) that are defined on the [Wire](creating-components.md). Optional array of parameters can be provided, which will be passed on to listeners.

```javascript
wire( "TaskList" ).emit( "addedTask" );
wire( "TaskList" ).emit( "addedTask", [ param1, param2 ] );
```

### see

Verifies a value can be found in the current [Template](templates.md) rendering. Otherwise, test fails.

```javascript
wire( "TaskList" ).see( "<h1>My Tasks</h1>" );
```

### dontSee

Verifies a value is not found in the current [Template](templates.md) rendering. Otherwise, test fails.

```javascript
wire( "TaskList" ).dontSee( "<h1>Someone Else's Tasks</h1> ");
```

### seeData

Verifies a [Data Property](properties.md) matches a specified value. Otherwise, test fails.

```javascript
wire( "TaskList" ).seeData( "tasksRemoved", true );
```

### dontSeeData

Verifies a [Data Property](properties.md) does not match a specified value. Otherwise, test fails.

```javascript
wire( "TaskList" ).dontSeeData( "tasksRemoved", true );
```
