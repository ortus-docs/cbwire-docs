# What's New With 3.2

12/20/2023

## New Features

### Auto-populate Data Properties

CBWIRE 3.2 introduces automatic data property population when `onMount()` isn't defined. This streamlines component development by reducing boilerplate code for simple components.

```javascript
// wires/SimpleProfile.cfc
component extends="cbwire.models.Component" {
    // No need to define onMount() - data properties are auto-populated
    data = {
        "name" = "",
        "email" = "",
        "bio" = ""
    };
    
    function updateProfile() {
        // Save profile logic
        saveUserProfile(data);
    }
}
```

Previously, you needed to explicitly define `onMount()` to ensure data properties were available. Now, CBWIRE automatically handles this initialization for you when passing parameters to components without an `onMount()` method.

### Module-Specific Component Loading

Support for `wire("MyComponent@module")` calls enables loading components from specific modules, improving organization in complex applications.

```html
<!-- Load component from specific module -->
#wire("UserDashboard@admin")#

<!-- Load with parameters from module -->
#wire("ReportGenerator@reporting", { "type": "monthly" })#

<!-- Load from nested folders within modules -->
#wire("myComponents.NestedComponent@testingmodule")#

<!-- Standard component loading still works -->
#wire("StandardComponent")#
```

```javascript
// modules/admin/wires/UserDashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "users" = [],
        "totalUsers" = 0
    };
    
    function onMount() {
        data.users = getUserList();
        data.totalUsers = arrayLen(data.users);
    }
}
```

### Browser Event Dispatching

CBWIRE 3.2 adds support for dispatching browser events using the new `dispatch()` method, enabling better integration with JavaScript frameworks and third-party libraries.

```javascript
// wires/NotificationCenter.cfc
component extends="cbwire.models.Component" {
    data = {
        "notifications" = []
    };
    
    function addNotification(message) {
        arrayAppend(data.notifications, {
            "id" = createUUID(),
            "message" = message,
            "timestamp" = now()
        });
        
        // Dispatch browser event
        dispatch("notification-added", {
            "count" = arrayLen(data.notifications),
            "latest" = message
        });
    }
    
    function clearNotifications() {
        data.notifications = [];
        dispatch("notifications-cleared");
    }
}
```

```javascript
// Listen for events in your JavaScript
document.addEventListener('notification-added', function(event) {
    console.log('New notification:', event.detail.latest);
    updateNotificationBadge(event.detail.count);
});

document.addEventListener('notifications-cleared', function(event) {
    resetNotificationBadge();
});
```

## Enhancements

### Improved Component Initialization

* Streamlined data property setup for components without custom initialization logic
* Reduced boilerplate code requirements for simple components
* Better parameter handling with type validation for security

### Module System Integration

* Enhanced module loading capabilities with `@module` syntax
* Support for nested folders within modules
* Better organization for large applications with multiple modules
* Seamless integration with existing ColdBox module architecture

### Template Access Improvements

* Direct access to ColdBox event object in templates via `event` variable
* Ability to call event methods directly from templates
* Enhanced integration between CBWIRE and ColdBox framework

## Breaking Changes

This release maintains backward compatibility with existing CBWIRE 3.x applications. No breaking changes were introduced in version 3.2.

## Contributors

Special thanks to **Michael Rigsby** for contributing the auto-populate data properties feature and module-specific component loading functionality.
