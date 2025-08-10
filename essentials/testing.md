---
description: Test your components using TestBox.
---

# Testing

CBWIRE includes a beautiful test API to test your front-end components quickly.

## Test Example

{% hint style="info" %}
To start, make sure you extend _BaseWireTest_ in your test spec.
{% endhint %}

You can invoke your component by calling **wire()** and then chain additional state changes and assertions.

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

Set a data property to the specified value.

```javascript
wire( "TaskList" )
    .data( "tasks", [ "task1", "task2" ] ) // one property at a time
    .data( { "tasks" : [ "task1", "task2" ] } ); // struct of properties
```

### computed

Set a computed property to the specified closure.

```javascript
wire( "TaskList" )
    .computed( "count", function() { return 3; } ) // one property at a time
    .computed( { "count" : function() { return 3; } } ); // struct of properties
```

### toggle

Toggles a data property between true and false.

```javascript
wire( "TaskList" ).toggle( "showModal" );
```

### call

Calls an action. An optional array of parameters can be provided.

```javascript
wire( "TaskList" ).call( "clearTasks" );
wire( "TaskList" ).call( "clearTasks", [ param1, param2 ] );
```

### emit

Emits an event and fires any listeners that are defined on the component. An optional array of parameters can be provided, which will be passed on to listeners.

```javascript
wire( "TaskList" ).emit( "addedTask" );
wire( "TaskList" ).emit( "addedTask", [ param1, param2 ] );
```

### see

Verifies a value can be found in the current component rendering. Otherwise, the test fails.

```javascript
wire( "TaskList" ).see( "<h1>My Tasks</h1>" );
```

### dontSee

Verifies a value is not found in the current component rendering. Otherwise, the test fails.

```javascript
wire( "TaskList" ).dontSee( "<h1>Someone Else's Tasks</h1> ");
```

### seeData

Verifies a data property matches a specified value. Otherwise, the test fails.

```javascript
wire( "TaskList" ).seeData( "tasksRemoved", true );
```

### dontSeeData

Verifies a data property does not match a specified value. Otherwise, test fails.

```javascript
wire( "TaskList" ).dontSeeData( "tasksRemoved", true );
```
