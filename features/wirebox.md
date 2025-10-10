# WireBox

CBWIRE seamlessly integrates with WireBox, ColdBox's powerful dependency injection framework, allowing you to inject services, models, and other dependencies directly into your components. This separation of concerns keeps business logic out of your components and promotes clean, maintainable code architecture.

## Basic Dependency Injection

The most common way to access dependencies is through property injection using the `inject` attribute:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// models/UserService.bx
class singleton {
    function getAll() {
        return queryExecute("select id, name, email from users");
    }
    
    function create(userData) {
        return queryExecute(
            "insert into users (name, email) values (:name, :email)",
            {
                name: {value: userData.name, cfsqltype: "cf_sql_varchar"},
                email: {value: userData.email, cfsqltype: "cf_sql_varchar"}
            }
        );
    }
    
    function delete(userId) {
        queryExecute(
            "delete from users where id = :id", 
            {id: {value: userId, cfsqltype: "cf_sql_integer"}}
        );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// models/UserService.cfc
component singleton {
    function getAll() {
        return queryExecute("select id, name, email from users");
    }
    
    function create(userData) {
        return queryExecute(
            "insert into users (name, email) values (:name, :email)",
            {
                name: {value: userData.name, cfsqltype: "cf_sql_varchar"},
                email: {value: userData.email, cfsqltype: "cf_sql_varchar"}
            }
        );
    }
    
    function delete(userId) {
        queryExecute(
            "delete from users where id = :id", 
            {id: {value: userId, cfsqltype: "cf_sql_integer"}}
        );
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserManager.bx
class extends="cbwire.models.Component" {
    // Inject dependencies via properties
    property name="userService" inject="UserService";
    property name="logger" inject="logbox:logger:{this}";
    
    data = {
        "users": [],
        "newUser": {"name": "", "email": ""}
    };

    function onMount() {
        data.users = userService.getAll();
        logger.info("UserManager component mounted");
    }

    function addUser() {
        if (data.newUser.name.len() && data.newUser.email.len()) {
            userService.create(data.newUser);
            data.users = userService.getAll(); // Refresh list
            data.newUser = {"name": "", "email": ""}; // Reset form
            logger.info("User created: #data.newUser.name#");
        }
    }

    function deleteUser(userId) {
        userService.delete(userId);
        data.users = userService.getAll(); // Refresh list
        logger.info("User deleted: #userId#");
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserManager.cfc
component extends="cbwire.models.Component" {
    // Inject dependencies via properties
    property name="userService" inject="UserService";
    property name="logger" inject="logbox:logger:{this}";
    
    data = {
        "users" = [],
        "newUser" = {"name" = "", "email" = ""}
    };

    function onMount() {
        data.users = userService.getAll();
        logger.info("UserManager component mounted");
    }

    function addUser() {
        if (len(data.newUser.name) && len(data.newUser.email)) {
            userService.create(data.newUser);
            data.users = userService.getAll(); // Refresh list
            data.newUser = {"name" = "", "email" = ""}; // Reset form
            logger.info("User created: #data.newUser.name#");
        }
    }

    function deleteUser(userId) {
        userService.delete(userId);
        data.users = userService.getAll(); // Refresh list
        logger.info("User deleted: #userId#");
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userManager.bxm -->
<bx:output>
<div>
    <h1>User Management</h1>
    
    <!-- Add new user form -->
    <form wire:submit="addUser">
        <input type="text" wire:model="newUser.name" placeholder="Name" required>
        <input type="email" wire:model="newUser.email" placeholder="Email" required>
        <button type="submit">Add User</button>
    </form>
    
    <!-- User list -->
    <div class="user-list">
        <bx:loop array="#users#" index="user">
            <div wire:key="user-#user.id#" class="user-item">
                <span>#user.name# (#user.email#)</span>
                <button wire:click="deleteUser(#user.id#)" wire:confirm="Delete this user?">
                    Delete
                </button>
            </div>
        </bx:loop>
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userManager.cfm -->
<cfoutput>
<div>
    <h1>User Management</h1>
    
    <!-- Add new user form -->
    <form wire:submit="addUser">
        <input type="text" wire:model="newUser.name" placeholder="Name" required>
        <input type="email" wire:model="newUser.email" placeholder="Email" required>
        <button type="submit">Add User</button>
    </form>
    
    <!-- User list -->
    <div class="user-list">
        <cfloop array="#users#" index="user">
            <div wire:key="user-#user.id#" class="user-item">
                <span>#user.name# (#user.email#)</span>
                <button wire:click="deleteUser(#user.id#)" wire:confirm="Delete this user?">
                    Delete
                </button>
            </div>
        </cfloop>
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Dynamic Instance Retrieval

For cases where you need to retrieve dependencies dynamically within action methods, use the `getInstance()` method:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ReportGenerator.bx
class extends="cbwire.models.Component" {
    data = {
        "reportType": "",
        "reports": []
    };

    function generateReport() {
        // Dynamically get the appropriate service based on report type
        var serviceName = data.reportType & "ReportService";
        var reportService = getInstance(serviceName);
        
        data.reports = reportService.generate();
    }

    function exportData(format) {
        // Get exporter based on format
        var exporter = getInstance("exporters.#format#Exporter");
        return exporter.export(data.reports);
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ReportGenerator.cfc
component extends="cbwire.models.Component" {
    data = {
        "reportType" = "",
        "reports" = []
    };

    function generateReport() {
        // Dynamically get the appropriate service based on report type
        var serviceName = data.reportType & "ReportService";
        var reportService = getInstance(serviceName);
        
        data.reports = reportService.generate();
    }

    function exportData(format) {
        // Get exporter based on format
        var exporter = getInstance("exporters.#format#Exporter");
        return exporter.export(data.reports);
    }
}
```
{% endtab %}
{% endtabs %}

## Common Injection Patterns

### Service Layer Injection

```javascript
property name="userService" inject="UserService";
property name="emailService" inject="EmailService";
property name="cacheService" inject="CacheService";
```

### LogBox Logger Injection

```javascript
property name="logger" inject="logbox:logger:{this}";
```

### ColdBox Settings Injection

```javascript
property name="settings" inject="coldbox:setting:myCustomSettings";
property name="appSettings" inject="coldbox:fwSetting:applicationName";
```

### Custom Provider Injection

```javascript
property name="apiClient" inject="provider:APIClientFactory";
property name="configService" inject="id:ConfigurationService";
```

## getInstance() Method Signature

The `getInstance()` method provides flexible dependency retrieval:

```javascript
/**
 * Get an instance object from WireBox
 *
 * @name The mapping name or CFC path or DSL to retrieve
 * @initArguments The constructor structure of arguments to passthrough when initializing the instance
 * @dsl The DSL string to use to retrieve an instance
 *
 * @return The requested instance
 */
function getInstance(name, initArguments={}, dsl)
```

### Usage Examples

```javascript
// Basic retrieval
var userService = getInstance("UserService");

// With initialization arguments
var apiClient = getInstance("APIClient", {
    "baseURL": "https://api.example.com",
    "timeout": 30
});

// Using DSL
var logger = getInstance(dsl="logbox:logger:MyComponent");
```

## Lifecycle Integration

CBWIRE components fully participate in WireBox's lifecycle management. All WireBox lifecycle methods are available:

```javascript
// WireBox lifecycle methods available in CBWIRE components
function onDIComplete() {
    // Called after all dependencies are injected
}

function postInit() {
    // Called after object initialization
}

function preDestroy() {
    // Called before object destruction
}
```

## Benefits of WireBox Integration

* **Separation of Concerns**: Keep business logic in dedicated service layers
* **Testability**: Easily mock dependencies for unit testing
* **Flexibility**: Switch implementations via WireBox mappings
* **Performance**: Leverage WireBox's singleton and caching capabilities
* **Configuration**: Externalize configuration through WireBox DSL

{% hint style="success" %}
Using WireBox with CBWIRE promotes clean architecture by separating presentation logic (components) from business logic (services), making your application more maintainable and testable.
{% endhint %}

{% hint style="info" %}
CBWIRE uses WireBox internally to instantiate your components, so all WireBox lifecycle methods and features are automatically available to your components.
{% endhint %}

## Further Documentation

Learn about the full capabilities of WireBox at [https://wirebox.ortusbooks.com/](https://wirebox.ortusbooks.com/).
