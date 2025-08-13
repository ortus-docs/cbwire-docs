# How It Works

CBWIRE bridges the gap between server-side BoxLang/CFML code and dynamic frontend experiences. Understanding how this magic happens will help you build better applications and debug issues when they arise.

## The CBWIRE Architecture

CBWIRE consists of four main parts working together:

1. **Server-side Components** - Your BoxLang/CFML classes that handle data and business logic
2. **Livewire.js** - Client-side JavaScript that manages DOM updates and server communication
3. **Alpine.js** - Lightweight JavaScript framework for client-side interactivity and state management
4. **ColdBox Integration** - The glue that connects everything within the ColdBox framework

## Component Lifecycle

Let's trace through what happens when a CBWIRE component is rendered and interacted with:

### Initial Page Load

When a user first requests a page containing CBWIRE components:

1. **Component Instantiation**: CBWIRE creates an instance of your component class
2. **Data Initialization**: The `data` struct is populated with default values
3. **Mount Lifecycle**: If present, the `onMount()` method executes
4. **Template Rendering**: Your component's template is rendered with current data values
5. **HTML Generation**: The final HTML is sent to the browser with embedded metadata

```html
<!-- Example rendered output -->
<div wire:id="abc123" wire:data='{"counter":0}'>
    <h2>Counter: 0</h2>
    <button wire:click="increment">+</button>
</div>
```

### User Interaction Flow

When a user interacts with your component (clicks a button, submits a form, etc.):

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

1. **Event Capture**: Livewire.js intercepts the DOM event (click, input change, form submit)
2. **Request Preparation**: The client serializes current component state and the action to perform
3. **Server Request**: An AJAX request is sent to CBWIRE's update endpoint
4. **Component Hydration**: CBWIRE recreates your component instance from the serialized state
5. **Action Execution**: Your component method runs with the current data context
6. **Data Updates**: Component data properties are modified by your action method
7. **Re-rendering**: The template is re-rendered with updated data
8. **Response Generation**: New HTML and updated state are sent back to the client
9. **DOM Diffing**: Livewire.js compares old and new HTML, updating only changed elements
10. **UI Update**: The page updates without a full refresh

## Request Anatomy

Here's what a typical CBWIRE request looks like:

### Outgoing Request
```javascript
// POST /cbwire/updates
{
    "fingerprint": {
        "id": "abc123",
        "name": "counter",
        "locale": "en",
        "path": "/",
        "method": "GET"
    },
    "serverMemo": {
        "data": {"counter": 5},
        "checksum": "xyz789"
    },
    "updates": [
        {
            "type": "callMethod",
            "payload": {
                "method": "increment",
                "params": []
            }
        }
    ]
}
```

### Server Response
```javascript
{
    "effects": {
        "html": "<div wire:id=\"abc123\"...>...</div>",
        "dirty": ["counter"]
    },
    "serverMemo": {
        "data": {"counter": 6},
        "checksum": "abc456"
    }
}
```

## Data Binding Deep Dive

CBWIRE provides several ways to bind data between your template and component:

### One-Way Data Binding

Data flows from component to template automatically:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserProfile.bx
class extends="cbwire.models.Component" {
    data = {
        "user": {
            "name": "John Doe",
            "email": "john@example.com"
        }
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    data = {
        "user" = {
            "name" = "John Doe",
            "email" = "john@example.com"
        }
    };
}
```
{% endtab %}
{% endtabs %}

```html
<!-- Template automatically reflects data changes -->
<div>
    <h1>Welcome, #data.user.name#</h1>
    <p>Email: #data.user.email#</p>
</div>
```

### Two-Way Data Binding

Form inputs can automatically sync with component data:

```html
<!-- Changes to input automatically update component data -->
<input type="text" wire:model="user.name" />
<input type="email" wire:model="user.email" />
```

When users type in these fields, CBWIRE automatically updates your component's data properties in real-time.

## Action Methods

Actions are public methods in your component that can be called from the template:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/TodoList.bx
class extends="cbwire.models.Component" {
    data = {
        "todos": [],
        "newTodo": ""
    };
    
    function addTodo() {
        if (len(trim(variables.data.newTodo))) {
            arrayAppend(variables.data.todos, {
                "id": createUUID(),
                "text": variables.data.newTodo,
                "completed": false
            });
            variables.data.newTodo = "";
        }
    }
    
    function toggleTodo(todoId) {
        for (var todo in variables.data.todos) {
            if (todo.id == arguments.todoId) {
                todo.completed = !todo.completed;
                break;
            }
        }
    }
    
    function removeTodo(todoId) {
        variables.data.todos = arrayFilter(variables.data.todos, function(todo) {
            return todo.id != todoId;
        });
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/TodoList.cfc
component extends="cbwire.models.Component" {
    data = {
        "todos" = [],
        "newTodo" = ""
    };
    
    function addTodo() {
        if (len(trim(variables.data.newTodo))) {
            arrayAppend(variables.data.todos, {
                "id" = createUUID(),
                "text" = variables.data.newTodo,
                "completed" = false
            });
            variables.data.newTodo = "";
        }
    }
    
    function toggleTodo(todoId) {
        for (var todo in variables.data.todos) {
            if (todo.id == arguments.todoId) {
                todo.completed = !todo.completed;
                break;
            }
        }
    }
    
    function removeTodo(todoId) {
        variables.data.todos = arrayFilter(variables.data.todos, function(todo) {
            return todo.id != todoId;
        });
    }
}
```
{% endtab %}
{% endtabs %}

```html
<!-- Template calling various action methods -->
<div>
    <input type="text" wire:model="newTodo" placeholder="Add a todo...">
    <button wire:click="addTodo">Add</button>
    
    <ul>
        <li wire:key="todo-#todo.id#" wire:each="todo in todos">
            <input type="checkbox" wire:click="toggleTodo('#todo.id#')" #todo.completed ? 'checked' : ''#>
            <span class="#todo.completed ? 'completed' : ''#">#todo.text#</span>
            <button wire:click="removeTodo('#todo.id#')">Delete</button>
        </li>
    </ul>
</div>
```

## State Management

CBWIRE automatically manages component state across requests:

### State Serialization
- Component data is serialized and sent with each request
- Only public data properties are included in the state
- Private variables and computed properties are not persisted

### State Security
- A checksum is generated to prevent tampering
- Modified checksums result in request rejection
- Sensitive data should be stored server-side (session, cache, database)

### State Optimization
```javascript
// Exclude large or sensitive data from client state
excludeFromClientState = ["largeDataSet", "sensitiveInfo"];
```

## Performance Considerations

### Request Optimization
- CBWIRE only sends changed data properties
- DOM diffing updates only modified elements
- Multiple rapid interactions are debounced

### Memory Management
- Component instances are created fresh for each request
- No server-side state is maintained between requests
- Use appropriate caching strategies for expensive operations

## Security Model

CBWIRE includes several security features:

### CSRF Protection
- Optional CSRF tokens prevent cross-site request forgery
- Tokens are automatically managed when enabled

### Method Authorization
```javascript
// Only public methods can be called from templates
public function allowedAction() { }

private function protectedAction() { } // Cannot be called via wire:click
```

### Data Validation
```javascript
function updateProfile(name, email) {
    // Always validate input data
    if (!isValid("email", arguments.email)) {
        throw("Invalid email address");
    }
    // Process validated data
}
```

## Debugging CBWIRE Applications

### Common Issues and Solutions

**Component not updating?**
- Check browser console for JavaScript errors
- Verify CBWIRE assets are loaded
- Ensure action methods are public

**Data not persisting?**
- Remember: each request creates a fresh component instance
- Use session, cache, or database for persistent data
- Check if data properties are being overwritten

**Performance problems?**
- Minimize data sent to client
- Use `wire:key` for list items
- Avoid heavy computations in templates

### Development Tools
- Browser developer tools show CBWIRE requests
- Server logs contain component errors
- ColdBox debugger shows component lifecycle

## Advanced Concepts

### Component Communication
Components can communicate through events:

```javascript
// Emit events from one component
emit("userUpdated", {"userId": 123});

// Listen for events in other components
listeners = {
    "userUpdated": "handleUserUpdate"
};

function handleUserUpdate(data) {
    // React to user update
}
```

### Alpine.js Integration
Combine CBWIRE with Alpine.js for optimal UX:

```html
<!-- Alpine handles client-side interactions -->
<div x-data="{ count: $wire.counter }">
    <span x-text="count"></span>
    <button @click="count++" x-on:click.debounce.500ms="$wire.save(count)">+</button>
</div>
```

### Custom Directives
Extend CBWIRE with custom behavior:

```html
<!-- Custom loading states -->
<div wire:loading.class="opacity-50">
    Content becomes transparent while updating
</div>

<!-- Conditional visibility -->
<div wire:loading wire:target="save">
    Saving...
</div>
```

Understanding these concepts will help you build robust, efficient CBWIRE applications that provide excellent user experiences while maintaining clean, server-side code.