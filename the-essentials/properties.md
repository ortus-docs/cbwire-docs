# Data Properties

Data Properties hold the state of your component and are defined with a **data** structure. Each data property is assigned a default value and is automatically synchronized between server and client.

Data properties are reactive - when an action modifies a property, CBWIRE automatically re-renders only the affected parts of your template. They can hold strings, numbers, booleans, dates, arrays, and structures, and are directly accessible in templates without special syntax.

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

Locked properties prevent client-side modifications to sensitive data like user IDs, roles, or permissions. Define a `locked` variable to protect specific properties from `wire:model` and other client interactions. CBWIRE throws an exception if locked properties are modified from the client.

### Single Property

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

### Multiple Properties

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

### Template Usage

Locked properties can be displayed but not modified through form inputs:

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

* **User Management**: Lock `userId`, `role`, `permissions`
* **E-commerce**: Lock product IDs, prices, inventory counts
* **Financial**: Lock account numbers, balances, transaction IDs
* **Content**: Lock author IDs, creation dates, publication status

{% hint style="info" %}
Locked properties only prevent client-side modifications. Server-side code in your action methods can still modify locked properties when necessary.
{% endhint %}

{% hint style="warning" %}
Remember that locked properties are still visible in the client-side JavaScript. For truly sensitive data that shouldn't be visible to users, avoid including it in data properties altogether and access it through server-side services instead.
{% endhint %}
