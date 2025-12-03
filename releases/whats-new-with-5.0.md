# What's New With 5.0

[Release Date TBD]

## Enhancements

### Livewire v3.6.4

Upgraded the underlying Livewire JavaScript to v3.6.4, bringing new features including:

- **`wire:current`** - Directive allows you to easily detect and style currently active links on a page
- **`wire:cloak`** - Hides elements until Livewire initializes, preventing flash of unstyled content during page load
- **`wire:show`** - Toggle element visibility using CSS without removing elements from the DOM, enabling smooth transitions
- **`wire:text`** - Dynamically update element text content without network roundtrips, perfect for optimistic UIs
- **`wire:replace`** - Force elements to render from scratch instead of DOM diffing, useful for third-party libraries and web components

### BoxLang Support

Full BoxLang support with enhanced performance and modern language features. Components use `.bx` for classes and `.bxm` for templates. All documentation includes BoxLang examples alongside CFML.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserDashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "users": [],
        "searchTerm": ""
    };

    function onMount() {
        data.users = getUserService().getAllUsers();
    }

    function filteredUsers() computed {
        return data.users.filter(function(user) {
            return !data.searchTerm.len() ||
                   user.name.findNoCase(data.searchTerm);
        });
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserDashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "users" = [],
        "searchTerm" = ""
    };

    function onMount() {
        data.users = getUserService().getAllUsers();
    }

    function filteredUsers() computed {
        return arrayFilter(data.users, function(user) {
            return !len(data.searchTerm) ||
                   findNoCase(data.searchTerm, user.name);
        });
    }
}
```
{% endtab %}
{% endtabs %}

### Upload Error Handling

The `onUploadError()` lifecycle hook provides graceful error handling for failed uploads. In 4.x, upload errors threw exceptions.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/PhotoUpload.bx
class extends="cbwire.models.Component" {
    data = {
        "photo": "",
        "uploadFailed": false,
        "errorMessage": ""
    };

    function onUploadError( property, errors, multiple ) {
        data.uploadFailed = true;
        data.errorMessage = "Failed to upload " & ( multiple ? "files" : "file" );

        if ( !isNull( errors ) ) {
            writeLog( type="error", text="Upload error for #property#: #serializeJSON(errors)#" );
        }
    }

    function save() {
        if ( data.photo != "" ) {
            var uploadPath = expandPath( "./uploads/#createUUID()#.jpg" );
            fileWrite( uploadPath, data.photo.get() );
            data.photo.destroy();
            data.photo = "";
            data.uploadFailed = false;
        }
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/PhotoUpload.cfc
component extends="cbwire.models.Component" {
    data = {
        "photo" = "",
        "uploadFailed" = false,
        "errorMessage" = ""
    };

    function onUploadError( property, errors, multiple ) {
        data.uploadFailed = true;
        data.errorMessage = "Failed to upload " & ( multiple ? "files" : "file" );

        if ( !isNull( errors ) ) {
            writeLog( type="error", text="Upload error for #property#: #serializeJSON(errors)#" );
        }
    }

    function save() {
        if ( data.photo != "" ) {
            var uploadPath = expandPath( "./uploads/#createUUID()#.jpg" );
            fileWrite( uploadPath, data.photo.get() );
            data.photo.destroy();
            data.photo = "";
            data.uploadFailed = false;
        }
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/photoUpload.bxm -->
<bx:output>
<div>
    <h1>Upload Photo</h1>

    <form wire:submit.prevent="save">
        <input type="file" wire:model="photo" accept="image/*">

        <bx:if uploadFailed>
            <div class="alert alert-danger">
                #errorMessage#
            </div>
        </bx:if>

        <button type="submit">Save Photo</button>
    </form>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/photoUpload.cfm -->
<cfoutput>
<div>
    <h1>Upload Photo</h1>

    <form wire:submit.prevent="save">
        <input type="file" wire:model="photo" accept="image/*">

        <cfif uploadFailed>
            <div class="alert alert-danger">
                #errorMessage#
            </div>
        </cfif>

        <button type="submit">Save Photo</button>
    </form>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

The hook receives `property` (string), `errors` (any), and `multiple` (boolean) parameters. Triggered when uploads fail due to HTTP errors, network issues, or server unavailability.

See the [File Upload Error Handling](../features/file-uploads.md#handling-upload-errors) documentation for details.

### Event Object Access

Access the ColdBox event object (request context) directly in templates to use methods like `event.buildLink()` for generating URLs.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/navigation.bxm -->
<bx:output>
<nav>
    <ul>
        <li><a href="#event.buildLink('dashboard')#">Dashboard</a></li>
        <li><a href="#event.buildLink('profile')#">Profile</a></li>
        <li><a href="#event.buildLink('settings')#">Settings</a></li>
    </ul>
</nav>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/navigation.cfm -->
<cfoutput>
<nav>
    <ul>
        <li><a href="#event.buildLink('dashboard')#">Dashboard</a></li>
        <li><a href="#event.buildLink('profile')#">Profile</a></li>
        <li><a href="#event.buildLink('settings')#">Settings</a></li>
    </ul>
</nav>
</cfoutput>
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Background requests to `/cbwire/update` may not have access to RC/PRC values set by interceptors or handlers. Store needed values as data properties in `onMount()` to ensure they persist across re-renders.
{% endhint %}

### Dot Notation Data Properties

Work with nested data structures using dot-separated paths in `wire:model` and component code. Reference hierarchical data naturally without flattening your structure.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserProfile.bx
class extends="cbwire.models.Component" {
    data = {
        "user": {
            "name": {
                "first": "John",
                "last": "Doe"
            },
            "contact": {
                "email": "john@example.com",
                "phone": "555-1234"
            }
        }
    };

    function onUpdateuser_name_first( value, oldValue ) {
        writeLog( "First name changed from #oldValue# to #value#" );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    data = {
        "user" = {
            "name" = {
                "first" = "John",
                "last" = "Doe"
            },
            "contact" = {
                "email" = "john@example.com",
                "phone" = "555-1234"
            }
        }
    };

    function onUpdateuser_name_first( value, oldValue ) {
        writeLog( "First name changed from #oldValue# to #value#" );
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userProfile.bxm -->
<bx:output>
<div>
    <input type="text" wire:model="user.name.first" placeholder="First Name">
    <input type="text" wire:model="user.name.last" placeholder="Last Name">
    <input type="email" wire:model="user.contact.email" placeholder="Email">
    <input type="tel" wire:model="user.contact.phone" placeholder="Phone">
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userProfile.cfm -->
<cfoutput>
<div>
    <input type="text" wire:model="user.name.first" placeholder="First Name">
    <input type="text" wire:model="user.name.last" placeholder="Last Name">
    <input type="email" wire:model="user.contact.email" placeholder="Email">
    <input type="tel" wire:model="user.contact.phone" placeholder="Phone">
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

Missing keys are created automatically. Lifecycle hooks use underscores—`user.name.first` triggers `onUpdateuser_name_first(newValue, oldValue)`.

### Single File Components

Define both template and logic in a single `.bxm` file. Use `<bx:output>` for HTML and `<bx:script>` with `// @startWire` and `// @endWire` comments for component code.

See the [Single-file Components](../features/single-file-components.md) documentation for details.


### Empty Component Error Messages

Clearer error messages for empty component templates. Contributed by **David Moreno**.

### Reduced DI Error Logging

Eliminated unnecessary "ioc.Injector" error messages during single file component initialization.

### Error Handling Improvements

Enhanced snapshot deserialization and effects processing. Invalid JSON now returns empty struct instead of throwing errors. Improved HTML entity encoding for content with quotes.

### Thread-Safe Component Building

Added locking mechanisms for thread-safe single file component building, preventing race conditions.

### External Module Support

Load wire components from external module locations via `modulesExternalLocation` configuration. Reference using `@module` syntax: `wire( name="MyComponent@externalModule" )`.

### CSRF Protection

Built-in Cross-Site Request Forgery protection prevents unauthorized requests. CSRF tokens are automatically generated and validated for all component interactions, with no changes required to component code.

**Session Storage by Default:**
CBWIRE 5.x uses session-based storage (OWASP-recommended), eliminating "Page Expired" errors common with cache-based approaches.

**Flexible Storage Options:**
Choose between `SessionCSRFStorage` (default) for single-server deployments or `CacheCSRFStorage` for distributed systems. Implement custom storage backends using the `ICSRFStorage` interface.

**Configuration:**
```javascript
moduleSettings = {
    cbwire = {
        csrfEnabled = true, // Enabled by default
        csrfStorage = "SessionCSRFStorage@cbwire"
    }
};
```

See the [Security](../features/security.md#csrf-protection) documentation for complete details.

## Breaking Changes

### Secure Upload Storage

File uploads now use the system temporary directory by default instead of the module's internal directory, preventing unauthorized access. Use the new `store()` method to move files to permanent storage.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/PhotoUpload.bx
class extends="cbwire.models.Component" {
    data = {
        "photo": ""
    };

    function save() {
        if (data.photo != "") {
            // New in 5.0: Use store() to move file to permanent location
            var storedPath = data.photo.store("/uploads/photos");

            // Process the stored file
            writeLog("Photo stored at: #storedPath#");

            // Clean up
            data.photo.destroy();
            data.photo = "";
        }
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/PhotoUpload.cfc
component extends="cbwire.models.Component" {
    data = {
        "photo" = ""
    };

    function save() {
        if (data.photo != "") {
            // New in 5.0: Use store() to move file to permanent location
            var storedPath = data.photo.store("/uploads/photos");

            // Process the stored file
            writeLog("Photo stored at: #storedPath#");

            // Clean up
            data.photo.destroy();
            data.photo = "";
        }
    }
}
```
{% endtab %}
{% endtabs %}

**Migration:** Replace `fileWrite(uploadPath, data.photo.get())` with `data.photo.store(path)`. The `store()` method is more efficient, creates directories automatically, and returns the stored file path.

See the [File Uploads](../features/file-uploads.md) documentation for details.

### Parameter Auto-Population

When a component defines `onMount()`, parameters passed via `wire()` are no longer automatically set to data properties. Explicitly assign parameters inside `onMount()`.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/ShowPost.bx
class extends="cbwire.models.Component" {
    data = {
        "title": "",
        "author": ""
    };

    function onMount( params ) {
        // Now required: explicitly set parameters when onMount() is defined
        data.title = params.title;
        data.author = params.author;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/ShowPost.cfc
component extends="cbwire.models.Component" {
    data = {
        "title" = "",
        "author" = ""
    };

    function onMount( params ) {
        // Now required: explicitly set parameters when onMount() is defined
        data.title = params.title;
        data.author = params.author;
    }
}
```
{% endtab %}
{% endtabs %}

**Migration:** Add explicit assignments in `onMount()`: `data.propertyName = params.propertyName`.

See the [Components](../the-essentials/components.md#auto-populating-data-properties) documentation for details.

### Engine Support Updates

Added Lucee 6.0+ support. Removed Adobe ColdFusion 2018 and 2021 as both have reached end-of-life.

**Supported engines:** BoxLang, Lucee 5.3+/6.0+, Adobe ColdFusion 2023+/2025+.

### ColdBox Support Updates

Added ColdBox 7+ and 8+ support. Supported versions: ColdBox 6+, 7+, 8+.

## Bug Fixes

### Upload Endpoint Configuration

File uploads now respect the `updateEndpoint` configuration setting instead of using hardcoded `/cbwire/upload` path.

### Upload Directory Path

Fixed incorrect path generation in `getUploadTempDirectory()` method.

### Directory Race Condition

Fixed race condition when multiple requests create temporary directory simultaneously. Added proper locking mechanisms.

### Lazy Loading Error

Fixed lazy loading attempting to call non-existent `onMount()` methods. Now checks for method existence before calling.

### Wires Location Config

Fixed `wiresLocation` configuration setting not working with custom values. Now properly applies custom locations.
