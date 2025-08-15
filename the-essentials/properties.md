# Data Properties

Data Properties hold the state of our component and are defined with a **data** structure in your [component](components.md). Each data property is assigned a default value.

Data properties are the foundation of state management in CBWIRE components. They represent the dynamic data that your component works with and can change over time as users interact with your application. These properties are automatically synchronized between the server and client, ensuring that your user interface always reflects the current state of your data.

When you define data properties in your component, CBWIRE makes them directly accessible in your templates without any special syntax - you can simply reference them by name. This creates a seamless development experience where your server-side data flows naturally into your presentation layer.

Data properties can hold various types of values including strings, numbers, booleans, dates, arrays, and structures. CBWIRE automatically handles the serialization and deserialization of these properties when communicating between the client and server, so you can focus on your application logic rather than data transformation concerns.

One of the most powerful aspects of data properties is their reactive nature. When an action method modifies a data property, CBWIRE automatically detects the change and re-renders only the parts of your template that depend on that property, creating an efficient and responsive user experience.

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
Data properties are visible to JavaScript. You SHOULD NOT store sensitive data in them. Use locked properties for additional protection against client-side modifications.
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
    resetExcept( "email" );
    resetExcept( [ "email", "phoneNumber" ] );
}
```

## Locked Properties

Locked properties provide an additional layer of security by preventing specific data properties from being modified through client-side interactions like `wire:model`, form submissions, or other user inputs. This is particularly useful for protecting sensitive identifiers, user roles, permissions, or any data that should only be modified by server-side logic.

When you define a property as locked, CBWIRE will throw a "Locked properties cannot be updated" exception if any client-side action attempts to modify it. This ensures that critical data remains secure even if malicious clients attempt to manipulate the data.

### Defining Locked Properties

You can lock properties by defining a `locked` variable in your component. This variable can be either a single property name (string) or an array of property names.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserProfile.bx
class extends="cbwire.models.Component" {
    // Lock single property
    locked = "userId";
    
    data = {
        "userId": 123,
        "name": "John Doe",
        "email": "john@example.com",
        "isActive": true
    };
    
    function updateProfile() {
        // Can modify name, email, isActive
        // Cannot modify userId - it's locked
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    // Lock single property
    locked = "userId";
    
    data = {
        "userId" = 123,
        "name" = "John Doe",
        "email" = "john@example.com",
        "isActive" = true
    };
    
    function updateProfile() {
        // Can modify name, email, isActive
        // Cannot modify userId - it's locked
    }
}
```
{% endtab %}
{% endtabs %}

### Locking Multiple Properties

For components that need to protect multiple sensitive properties, you can define an array of locked property names:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/AdminPanel.bx
class extends="cbwire.models.Component" {
    // Lock multiple properties
    locked = ["userId", "role", "permissions", "accountType"];
    
    data = {
        "userId": 123,
        "name": "Jane Admin",
        "email": "jane@company.com",
        "role": "administrator",
        "permissions": ["read", "write", "delete", "admin"],
        "accountType": "premium",
        "lastLogin": now(),
        "theme": "dark"
    };
    
    function updateSettings() {
        // Can modify: name, email, lastLogin, theme
        // Cannot modify: userId, role, permissions, accountType
        data.lastLogin = now();
        data.theme = "light";
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/AdminPanel.cfc
component extends="cbwire.models.Component" {
    // Lock multiple properties
    locked = ["userId", "role", "permissions", "accountType"];
    
    data = {
        "userId" = 123,
        "name" = "Jane Admin",
        "email" = "jane@company.com",
        "role" = "administrator",
        "permissions" = ["read", "write", "delete", "admin"],
        "accountType" = "premium",
        "lastLogin" = now(),
        "theme" = "dark"
    };
    
    function updateSettings() {
        // Can modify: name, email, lastLogin, theme
        // Cannot modify: userId, role, permissions, accountType
        data.lastLogin = now();
        data.theme = "light";
    }
}
```
{% endtab %}
{% endtabs %}

### Template Usage with Locked Properties

In your templates, you can still display locked properties and bind unlocked properties to form inputs. Locked properties remain readable but cannot be modified through client interactions:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/adminPanel.bxm -->
<bx:output>
<div>
    <h1>User Profile</h1>
    
    <!-- Display locked properties (read-only) -->
    <div class="readonly-info">
        <p><strong>User ID:</strong> #userId#</p>
        <p><strong>Role:</strong> #role#</p>
        <p><strong>Account Type:</strong> #accountType#</p>
    </div>
    
    <!-- Form with editable properties -->
    <form wire:submit="updateSettings">
        <!-- These inputs work normally -->
        <input type="text" wire:model="name" placeholder="Name">
        <input type="email" wire:model="email" placeholder="Email">
        
        <select wire:model="theme">
            <option value="light">Light</option>
            <option value="dark">Dark</option>
        </select>
        
        <!-- These would throw an exception if attempted -->
        <!-- <input type="hidden" wire:model="userId"> ❌ -->
        <!-- <input type="text" wire:model="role"> ❌ -->
        
        <button type="submit">Update Profile</button>
    </form>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/adminPanel.cfm -->
<cfoutput>
<div>
    <h1>User Profile</h1>
    
    <!-- Display locked properties (read-only) -->
    <div class="readonly-info">
        <p><strong>User ID:</strong> #userId#</p>
        <p><strong>Role:</strong> #role#</p>
        <p><strong>Account Type:</strong> #accountType#</p>
    </div>
    
    <!-- Form with editable properties -->
    <form wire:submit="updateSettings">
        <!-- These inputs work normally -->
        <input type="text" wire:model="name" placeholder="Name">
        <input type="email" wire:model="email" placeholder="Email">
        
        <select wire:model="theme">
            <option value="light">Light</option>
            <option value="dark">Dark</option>
        </select>
        
        <!-- These would throw an exception if attempted -->
        <!-- <input type="hidden" wire:model="userId"> ❌ -->
        <!-- <input type="text" wire:model="role"> ❌ -->
        
        <button type="submit">Update Profile</button>
    </form>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

### Common Use Cases

**User Management:**
- Lock `userId`, `role`, `permissions` while allowing profile updates
- Protect account type or subscription level from manipulation

**E-commerce:**
- Lock product IDs, prices, inventory counts in shopping cart components
- Protect order status or payment information

**Financial Applications:**
- Lock account numbers, balances, transaction IDs
- Protect audit trails and timestamps

**Content Management:**
- Lock author IDs, creation dates, publication status
- Protect content approval workflows

### Security Benefits

1. **Prevents Client-Side Manipulation**: Even if malicious users inspect and modify the DOM or network requests, locked properties cannot be changed
2. **Server-Side Validation**: CBWIRE validates locked properties on the server before processing any updates
3. **Exception Throwing**: Clear error messages when locked property modification is attempted
4. **Audit Trail Protection**: Ensures critical tracking data remains immutable from client interactions

{% hint style="info" %}
Locked properties only prevent client-side modifications. Server-side code in your action methods can still modify locked properties when necessary.
{% endhint %}

{% hint style="warning" %}
Remember that locked properties are still visible in the client-side JavaScript. For truly sensitive data that shouldn't be visible to users, avoid including it in data properties altogether and access it through server-side services instead.
{% endhint %}
