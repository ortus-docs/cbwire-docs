# What's New With 5.0

[Release Date TBD]

## Enhancements

### Livewire v3.6.4

Upgraded the underlying Livewire JavaScript to v3.6.4, bringing new features including:

- **`wire:current`** - Directive allows you to easily detect and style currently active links on a page

### BoxLang Support

Full support for BoxLang brings enhanced performance, improved syntax, and modern language features to your applications. All documentation has been updated with BoxLang examples alongside CFML examples. Components use the `.bx` extension for classes and `.bxm` extension for templates, with support for:

- Enhanced member functions (`.filter()`, `.map()`, `.reduce()`, `.each()`)
- Improved null handling with `isNull()` checks
- Modern class syntax with cleaner property definitions
- BoxLang-specific tags like `<bx:output>`, `<bx:if>`, `<bx:loop>`

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

### File Upload Error Handling

The new `onUploadError()` lifecycle hook provides graceful error handling for failed uploads, allowing you to display user-friendly error messages and implement custom recovery logic. In 4.x, upload errors would throw exceptions.

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

The `onUploadError()` hook receives three parameters:

| Parameter | Type    | Description                                                                                              |
| --------- | ------- | -------------------------------------------------------------------------------------------------------- |
| property  | string  | The name of the data property associated with the file input                                             |
| errors    | any     | The error response from the server. Will be null unless the HTTP status is 422, in which case it contains the response body |
| multiple  | boolean | Indicates whether multiple files were being uploaded (true) or a single file (false)                     |

The hook is triggered when:

* The upload server returns any HTTP status code outside the 2xx range (200-299)
* Network errors occur during upload
* The server is unreachable
* Any other upload failure scenario

See the [File Upload Error Handling](../features/file-uploads.md#handling-upload-errors) documentation for complete usage information.

### Event Object Access in Templates

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
**Important:** Be cautious when using `event.getCollection()` (RC scope) or `event.getPrivateCollection()` (PRC scope) in templates. Background requests to `/cbwire/update` may not have access to values set by interceptors or handlers that only run on your primary routes. Store needed values as data properties in your component's `onMount()` method to ensure they persist across re-renders.
{% endhint %}

### Single File BoxLang Components

Define both your component's template and logic in a single `.bxm` file for a more compact development experience.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/greeting.bxm -->
<bx:output>
<div>
    <h1>#title#</h1>
    <p>#message#</p>
    <button wire:click="updateMessage">Update Message</button>
</div>
</bx:output>

<bx:script>
// @startWire
data = {
    "title": "Welcome to CBWIRE",
    "message": "Hello from a single file component!"
};

function updateMessage() {
    data.message = "Message updated at " & now();
}
// @endWire
</bx:script>
```
{% endtab %}
{% endtabs %}

Single file components use two main sections:
- **`<bx:output>`**: Contains your component's HTML template
- **`<bx:script>`**: Contains your component's data properties and methods, wrapped in `// @startWire` and `// @endWire` comments

See the [Single-file Components](../features/single-file-components.md) documentation for complete usage information.

### Custom Temporary Storage Path

Configure a custom directory path for temporary file uploads using the new `storagePath` configuration setting. Particularly valuable in distributed server environments where temporary files need to be shared across multiple front-end servers.

Contributed by community member **David Moreno**.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        "storagePath": "/shared/temp/uploads"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "storagePath" = "/shared/temp/uploads"
    }
};
```
{% endtab %}
{% endtabs %}

See the [Configuration](../configuration.md#storagepath) and [File Uploads](../features/file-uploads.md) documentation for complete details.

### Improved Error Messaging for Empty Components

Clearer error messages when component templates are empty or missing HTML elements.

Contributed by community member **David Moreno**.

**Previous Error (4.x):**
```
Cannot return first element of array; array is empty
```

**New Error (5.0):**
```
The HTML content of the wire component must contain at least one external element.
Wire component contains no HTML elements. It is empty.
```

### Reduced Dependency Injection Error Logging

Eliminated unnecessary "ioc.Injector" error messages stating components were "not located in any declared scan location(s)" during single file component initialization, resulting in cleaner application logs.

### Enhanced Error Handling and Serialization

Improvements to error handling in snapshot deserialization and effects processing. Error handling has been refactored to remove exception causes from thrown exceptions, and effects deserialization now returns an empty struct for invalid JSON instead of throwing errors. HTML entity encoding has been improved to properly handle HTML content with quotes.

### Thread-Safe Single File Component Building

Added locking mechanisms to ensure thread-safe building of single file components, preventing race conditions in high-traffic environments.

### External Module Location Support

Load wire components from external module locations defined in your ColdBox configuration via `modulesExternalLocation`.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
class {
    function configure() {
        modulesExternalLocation = [
            "/modules_external"
        ];
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
component {
    function configure() {
        modulesExternalLocation = [
            "/modules_external"
        ];
    }
}
```
{% endtab %}
{% endtabs %}

Reference wires from external modules using the `@module` syntax:

```html
<div>
    #wire( name="MyComponent@externalModule" )#
</div>
```

### CSRF Protection

Cross-Site Request Forgery (CSRF) protection enhances security for your reactive applications. When enabled, all component actions require a valid CSRF token.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        "csrfEnabled": true
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "csrfEnabled" = true
    }
};
```
{% endtab %}
{% endtabs %}

CSRF protection is disabled by default but can be enabled via the `csrfEnabled` configuration setting. Customize the storage provider with the `csrfStorage` setting:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        "csrfEnabled": true,
        "csrfStorage": "CacheStorage@cbstorages"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "csrfEnabled" = true,
        "csrfStorage" = "CacheStorage@cbstorages"
    }
};
```
{% endtab %}
{% endtabs %}

The default storage provider is `SessionStorage@cbstorages`, but you can use any WireBox mapping that implements the appropriate storage interface.

See the [Configuration](../configuration.md#security-configuration) documentation for complete details.

## Breaking Changes

### Component Parameter Auto-Population

When a component defines an `onMount()` method, parameters passed via `wire()` are no longer automatically set to matching data properties. You must now explicitly assign parameters inside the `onMount()` method.

**Previous Behavior (4.x):**
Parameters would automatically populate data properties even when `onMount()` was defined.

**New Behavior (5.0):**
Parameters are only auto-populated when `onMount()` is NOT defined. If you define `onMount()`, you must manually set the parameters.

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

**Migration Guide:**

If your components define `onMount()` and rely on automatic parameter population, update them to explicitly assign parameters:

```javascript
function onMount( params ) {
    // Add explicit assignments for all parameters you need
    data.propertyName = params.propertyName;
}
```

See the [Components](../the-essentials/components.md#auto-populating-data-properties) documentation for complete details.

### CFML Engine Support Updates

Updated supported CFML engines to align with modern, actively maintained versions.

**Added Support:**
- Lucee 6.0+

**Removed Support:**
- Adobe ColdFusion 2018 (reached end-of-life)

**Supported Engines:**
- BoxLang
- Lucee 5.3+
- Lucee 6.0+
- Adobe ColdFusion 2021+
- Adobe ColdFusion 2023+
- Adobe ColdFusion 2025+

If you are still using Adobe ColdFusion 2018, remain on CBWIRE 4.x or upgrade to a supported CFML engine before upgrading to 5.0.

### Framework Support Updates

Added support for the latest ColdBox framework versions.

**Added Support:**
- ColdBox 7+
- ColdBox 8+

**Supported ColdBox Versions:**
- ColdBox 6+
- ColdBox 7+
- ColdBox 8+

## Bug Fixes

### File Upload Endpoint Configuration

Fixed file uploads to respect the `updateEndpoint` configuration setting. Previously, file uploads used a hardcoded `/cbwire/upload` path. File uploads now properly derive their endpoint from the `updateEndpoint` configuration.

### File Upload Temporary Directory Path

Fixed incorrect file path generation in the `getUploadTempDirectory()` method. The method was generating malformed paths like `cbwiremodels/tmp` instead of `cbwire/models/tmp`.

### Temporary Directory Race Condition

Fixed a race condition that caused errors when multiple requests attempted to create the temporary directory simultaneously. Added proper locking mechanisms to ensure thread-safe directory creation.

### Lazy Loading onMount Error

Fixed lazy loading attempting to call `onMount()` when the method didn't exist on the component. Now checks if the `onMount()` method exists before calling it.

### Wires Location Configuration

Fixed the `wiresLocation` configuration setting not being used when set to anything other than the default "wires" value. The configuration setting now properly applies custom wire component locations.
