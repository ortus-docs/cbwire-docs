# Security

CBWIRE provides multiple ways to secure your wire components, from simple authentication checks to integration with the cbSecurity module for role and permission-based authorization.

## onSecure() Lifecycle Method

The `onSecure()` lifecycle method runs before all other lifecycle methods, allowing you to enforce security rules at the earliest point in the request lifecycle. Return `false` to halt processing and render an empty div, or return nothing to continue processing normally.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/AdminDashboard.bx
class extends="cbwire.models.Component" {
    property name="authService" inject="AuthService";

    data = {
        "stats": {}
    };

    function onSecure( event, rc, prc, isInitial, params ) {
        // Check authentication
        if ( !authService.isLoggedIn() ) {
            return false;
        }

        // Check authorization
        if ( !authService.hasRole( "admin" ) ) {
            return false;
        }
    }

    function onMount() {
        data.stats = getAdminStats();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/AdminDashboard.cfc
component extends="cbwire.models.Component" {
    property name="authService" inject="AuthService";

    data = {
        "stats" = {}
    };

    function onSecure( event, rc, prc, isInitial, params ) {
        // Check authentication
        if ( !authService.isLoggedIn() ) {
            return false;
        }

        // Check authorization
        if ( !authService.hasRole( "admin" ) ) {
            return false;
        }
    }

    function onMount() {
        data.stats = getAdminStats();
    }
}
```
{% endtab %}
{% endtabs %}

### Parameters

| Parameter | Type    | Description                                                   |
| --------- | ------- | ------------------------------------------------------------- |
| event     | object  | The ColdBox request context                                   |
| rc        | struct  | Request collection                                            |
| prc       | struct  | Private request collection                                    |
| isInitial | boolean | True for initial mount, false for subsequent AJAX requests    |
| params    | struct  | Parameters passed to the component via `wire()`               |

### Custom Failure Messages

By default, returning `false` from `onSecure()` renders an empty div. Customize this message per component or globally:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/SecureContent.bx
class extends="cbwire.models.Component" {
    property name="authService" inject="AuthService";

    // Custom message when security check fails
    secureMountFailMessage = "<div class='alert alert-warning'>Please log in to view this content.</div>";

    data = {
        "content": ""
    };

    function onSecure( event, rc, prc, isInitial, params ) {
        if ( !authService.isLoggedIn() ) {
            return false;
        }
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/SecureContent.cfc
component extends="cbwire.models.Component" {
    property name="authService" inject="AuthService";

    // Custom message when security check fails
    secureMountFailMessage = "<div class='alert alert-warning'>Please log in to view this content.</div>";

    data = {
        "content" = ""
    };

    function onSecure( event, rc, prc, isInitial, params ) {
        if ( !authService.isLoggedIn() ) {
            return false;
        }
    }
}
```
{% endtab %}
{% endtabs %}

Set a global default in your ColdBox configuration:

```javascript
// config/ColdBox.cfc
moduleSettings = {
    cbwire = {
        secureMountFailMessage = "<div class='alert alert-danger'>Access Denied</div>"
    }
};
```

Component-level settings take precedence over module-level configuration.

## cbSecurity Integration

When the cbSecurity module is installed and active, CBWIRE automatically integrates with it, enabling security annotations on components and methods.

### Component-Level Security

Secure an entire component with the `secured` annotation:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/AdminPanel.bx
@secured
class extends="cbwire.models.Component" {
    data = {
        "users": []
    };

    function onMount() {
        data.users = userService.getAll();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/AdminPanel.cfc
component extends="cbwire.models.Component" secured {
    data = {
        "users" = []
    };

    function onMount() {
        data.users = userService.getAll();
    }
}
```
{% endtab %}
{% endtabs %}

### Method-Level Security

Secure specific actions with annotations:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserManagement.bx
class extends="cbwire.models.Component" {
    data = {
        "users": []
    };

    function onMount() {
        data.users = userService.getAll();
    }

    // Only users with "admin" permission can execute
    @secured("admin")
    function deleteUser( userId ) {
        userService.delete( userId );
        data.users = userService.getAll();
    }

    // Multiple permissions (user needs any one)
    @secured("admin,moderator")
    function updateUser( userId ) {
        userService.update( userId, data.users );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserManagement.cfc
component extends="cbwire.models.Component" {
    data = {
        "users" = []
    };

    function onMount() {
        data.users = userService.getAll();
    }

    // Only users with "admin" permission can execute
    function deleteUser( userId ) secured="admin" {
        userService.delete( userId );
        data.users = userService.getAll();
    }

    // Multiple permissions (user needs any one)
    function updateUser( userId ) secured="admin,moderator" {
        userService.update( userId, data.users );
    }
}
```
{% endtab %}
{% endtabs %}

### Lifecycle Method Security

Apply annotations to lifecycle methods:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Reports.bx
class extends="cbwire.models.Component" {
    data = {
        "reports": []
    };

    @secured("reports.view")
    function onMount() {
        data.reports = reportService.getAll();
    }

    @secured("reports.edit")
    function onUpdate() {
        // Validate and update
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Reports.cfc
component extends="cbwire.models.Component" {
    data = {
        "reports" = []
    };

    function onMount() secured="reports.view" {
        data.reports = reportService.getAll();
    }

    function onUpdate() secured="reports.edit" {
        // Validate and update
    }
}
```
{% endtab %}
{% endtabs %}

### Annotation Formats

cbSecurity annotations support multiple formats:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// Boolean - uses cbSecurity default rules
@secured
function someAction() {}

@secured(true)
function someAction() {}

// Single permission
@secured("admin")
function someAction() {}

// Multiple permissions (comma-separated)
@secured("admin,moderator,editor")
function someAction() {}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Boolean - uses cbSecurity default rules
function someAction() secured {}
function someAction() secured=true {}

// Single permission
function someAction() secured="admin" {}

// Multiple permissions (comma-separated)
function someAction() secured="admin,moderator,editor" {}
```
{% endtab %}
{% endtabs %}

## Security Interceptor

CBWIRE fires the `onCBWIRESecureFail` interception point when security checks fail, allowing custom handling like logging or redirects.

```javascript
// interceptors/SecurityLogger.cfc
component {
    function onCBWIRESecureFail( event, data, rc, prc ) {
        var componentName = data.componentName ?: "Unknown";
        var methodName = data.methodName ?: "N/A";
        var user = auth.user();

        writeLog(
            type = "warning",
            file = "security",
            text = "Security failure: User ##user.getId()## attempted to access #componentName#.#methodName#"
        );

        // Optional: redirect to login
        if ( !auth.check() ) {
            relocate( "login" );
        }
    }
}
```

Register the interceptor in your ColdBox configuration:

```javascript
// config/ColdBox.cfc
interceptors = [
    { class="interceptors.SecurityLogger" }
];
```

## Practical Examples

### Protecting Admin Features

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserList.bx
@secured("admin")
class extends="cbwire.models.Component" {
    property name="userService" inject="UserService";

    secureMountFailMessage = "<div class='alert alert-danger'>Admin access required</div>";

    data = {
        "users": [],
        "searchTerm": ""
    };

    function onMount() {
        data.users = userService.getAll();
    }

    @secured("admin.users.delete")
    function deleteUser( userId ) {
        userService.delete( userId );
        data.users = userService.getAll();
    }

    function filteredUsers() computed {
        return data.users.filter( function( user ) {
            return !data.searchTerm.len() ||
                   user.name.findNoCase( data.searchTerm );
        });
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserList.cfc
component extends="cbwire.models.Component" secured="admin" {
    property name="userService" inject="UserService";

    secureMountFailMessage = "<div class='alert alert-danger'>Admin access required</div>";

    data = {
        "users" = [],
        "searchTerm" = ""
    };

    function onMount() {
        data.users = userService.getAll();
    }

    function deleteUser( userId ) secured="admin.users.delete" {
        userService.delete( userId );
        data.users = userService.getAll();
    }

    function filteredUsers() computed {
        return data.users.filter( function( user ) {
            return !len( data.searchTerm ) ||
                   findNoCase( data.searchTerm, user.name );
        });
    }
}
```
{% endtab %}
{% endtabs %}

### Mixed Security Levels

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/BlogPost.bx
class extends="cbwire.models.Component" {
    property name="postService" inject="PostService";

    data = {
        "post": {},
        "comment": ""
    };

    function onMount( params ) {
        data.post = postService.get( params.postId );
    }

    // Anyone can add comments
    function addComment() {
        postService.addComment( data.post.id, data.comment );
        data.comment = "";
    }

    // Only editors can edit posts
    @secured("editor")
    function editPost() {
        postService.update( data.post );
    }

    // Only admins can delete
    @secured("admin")
    function deletePost() {
        postService.delete( data.post.id );
        redirect( "/blog" );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/BlogPost.cfc
component extends="cbwire.models.Component" {
    property name="postService" inject="PostService";

    data = {
        "post" = {},
        "comment" = ""
    };

    function onMount( params ) {
        data.post = postService.get( params.postId );
    }

    // Anyone can add comments
    function addComment() {
        postService.addComment( data.post.id, data.comment );
        data.comment = "";
    }

    // Only editors can edit posts
    function editPost() secured="editor" {
        postService.update( data.post );
    }

    // Only admins can delete
    function deletePost() secured="admin" {
        postService.delete( data.post.id );
        redirect( "/blog" );
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Security annotations are only active when cbSecurity is installed and configured. Without cbSecurity, use the `onSecure()` lifecycle method for custom security logic.
{% endhint %}

{% hint style="warning" %}
Security checks run on every request. For performance-critical components, consider caching authorization results or using efficient permission lookups.
{% endhint %}
