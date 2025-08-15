# Templates

Templates define your component's HTML presentation layer using standard HTML/CFML tags. They're dynamic, reactive views that access your component's data properties and methods to create interactive user experiences.

CBWIRE automatically looks for template files with the same name as your component. Templates can use BoxLang tags like `<bx:if>`, `<bx:loop>`, and `<bx:output>` or CFML tags like `<cfif>`, `<cfloop>`, and `<cfoutput>` alongside wire directives for reactive behavior.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/greeter.bxm -->
<bx:output>
<div>
    <h1>#greeting#</h1>
    <button wire:click="updateGreeting">Update</button>
    
    <bx:if timeOfDay() EQ "morning">
        <p>Good morning!</p>
    <bx:else>
        <p>Good day!</p>
    </bx:if>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/greeter.cfm -->
<cfoutput>
<div>
    <h1>#greeting#</h1>
    <button wire:click="updateGreeting">Update</button>
    
    <cfif timeOfDay() EQ "morning">
        <p>Good morning!</p>
    <cfelse>
        <p>Good day!</p>
    </cfif>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## File Structure

CBWIRE uses automatic file matching for components and templates:

```
./wires/Counter.bx     <!-- BoxLang component -->
./wires/Counter.cfc    <!-- CFML component -->
./wires/counter.bxm    <!-- BoxLang template -->
./wires/counter.cfm    <!-- CFML template -->
```

## Template Requirements

Templates must have a single outer element for proper DOM binding:

```html
<!-- ✅ Good: Single outer element -->
<div>
    <h1>My Component</h1>
    <p>Content here</p>
</div>

<!-- ❌ Bad: Multiple outer elements -->
<div>Component header</div>
<div>Component body</div>
```

## Data Properties

Access component data properties directly by name:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserCard.bx
class extends="cbwire.models.Component" {
    data = {
        "name": "John Doe",
        "email": "john@example.com",
        "isActive": true
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserCard.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "John Doe",
        "email" = "john@example.com",
        "isActive" = true
    };
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userCard.bxm -->
<bx:output>
<div class="user-card">
    <h2>#name#</h2>
    <p>Email: #email#</p>
    <bx:if isActive>
        <span class="active">Active User</span>
    </bx:if>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userCard.cfm -->
<cfoutput>
<div class="user-card">
    <h2>#name#</h2>
    <p>Email: #email#</p>
    <cfif isActive>
        <span class="active">Active User</span>
    </cfif>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Computed Properties

Define computed properties with the `computed` annotation and call them as methods:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Dashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "tasks": [
            {"id": 1, "completed": true},
            {"id": 2, "completed": false}
        ]
    };

    function completedCount() computed {
        return data.tasks.filter(function(task) {
            return task.completed;
        }).len();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Dashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "tasks" = [
            {"id" = 1, "completed" = true},
            {"id" = 2, "completed" = false}
        ]
    };

    function completedCount() computed {
        return arrayFilter(data.tasks, function(task) {
            return task.completed;
        }).len();
    }
}
```
{% endtab %}
{% endtabs %}

```html
<!-- Template usage -->
<div>
    <h1>Task Progress</h1>
    <p>Completed: #completedCount()# of #tasks.len()#</p>
</div>
```

## Explicit Templates

Override default template location using the `onRender()` method:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/CustomComponent.bx
class extends="cbwire.models.Component" {
    function onRender() {
        return template("shared.CustomLayout");
    }
    
    // With parameters
    function onRender() {
        return template("shared.UserCard", {
            "theme": "dark",
            "showAvatar": true
        });
    }
    
    // Inline template
    function onRender() {
        return "<div><h1>Simple Component</h1></div>";
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/CustomComponent.cfc
component extends="cbwire.models.Component" {
    function onRender() {
        return template("shared.CustomLayout");
    }
    
    // With parameters
    function onRender() {
        return template("shared.UserCard", {
            "theme" = "dark",
            "showAvatar" = true
        });
    }
    
    // Inline template
    function onRender() {
        return "<div><h1>Simple Component</h1></div>";
    }
}
```
{% endtab %}
{% endtabs %}

## Helper Methods

Access global helper methods from installed ColdBox modules:

```html
<!-- Using cbi18n module -->
<div>
    <h1>#$r("welcome.title")#</h1>
    <p>#$r("welcome.message")#</p>
</div>

<!-- Using cbstorages module -->
<div>
    <p>Session ID: #getSessionStorage().getId()#</p>
</div>
```

{% hint style="warning" %}
Templates must have a single outer element for CBWIRE's DOM diffing to work correctly.
{% endhint %}

{% hint style="info" %}
Computed properties are cached and only executed when first called or when their dependencies change. See [Computed Properties](computed-properties.md) for details.
{% endhint %}