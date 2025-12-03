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

{% hint style="warning" %}
`onSecure()` fires on every request—both initial rendering and all subsequent AJAX requests. This ensures security checks run continuously throughout the component's lifecycle.
{% endhint %}

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

## Checksum Validation

CBWIRE automatically validates component data integrity using HMAC-SHA256 checksums to prevent tampering with data between client and server. Every component snapshot includes a checksum that validates the data hasn't been modified.

### How It Works

When data travels between client and server, CBWIRE:

1. Calculates a cryptographic checksum of the component snapshot using HMAC-SHA256
2. Includes the checksum with the data payload
3. Validates the checksum on subsequent requests
4. Throws `CBWIRECorruptPayloadException` if validation fails

The checksum uses a secret key—either a custom secret you provide or a hash of the module's root path.

### Configuration

Control checksum validation in your ColdBox configuration:

```javascript
// config/ColdBox.cfc
moduleSettings = {
    cbwire = {
        // Disable checksum validation (not recommended for production)
        checksumValidation = false,

        // Optional: provide a custom secret key
        secret = "your-secret-key-here"
    }
};
```

{% hint style="danger" %}
Disabling checksum validation removes protection against client-side data tampering. Only disable this in controlled development environments where you trust all clients.
{% endhint %}

{% hint style="info" %}
If you don't provide a custom secret, CBWIRE generates one automatically based on the module's installation path. For production deployments across multiple servers, set a consistent secret key.
{% endhint %}

### Error Handling

Checksum validation failures throw `CBWIRECorruptPayloadException` in these scenarios:

- **Invalid JSON**: Payload is not properly formatted JSON
- **Missing Checksum**: No checksum found in the payload
- **Checksum Mismatch**: Calculated checksum doesn't match the provided checksum

These errors typically indicate:
- Client-side data tampering attempts
- Network corruption during transmission
- Version mismatches between client and server code
- Different secret keys across application servers

{% hint style="info" %}
Security annotations are only active when cbSecurity is installed and configured. Without cbSecurity, use the `onSecure()` lifecycle method for custom security logic.
{% endhint %}

{% hint style="warning" %}
Security checks run on every request. For performance-critical components, consider caching authorization results or using efficient permission lookups.
{% endhint %}
