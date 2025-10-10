# What's New With 5.0

[Release Date TBD]

## Enhancements

### BoxLang Support

CBWIRE 5.0 provides full support for BoxLang. BoxLang brings enhanced performance, improved syntax, and modern language features to your CBWIRE applications.

All CBWIRE documentation has been updated with BoxLang examples alongside CFML examples, making it easy to build reactive applications with BoxLang's powerful feature set. Components can use BoxLang's `.bx` extension for classes and `.bxm` extension for templates, with full support for BoxLang's modern syntax including:

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

CBWIRE 5.0 introduces the `onUploadError()` lifecycle hook, which is automatically called when a file upload encounters an error. This enhancement provides graceful error handling for failed uploads, allowing you to display user-friendly error messages and implement custom recovery logic.

In CBWIRE 4.x, upload errors would throw exceptions. With 5.0, you can handle these errors gracefully in your components.

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
        // Set error state
        data.uploadFailed = true;

        // Create user-friendly error message
        data.errorMessage = "Failed to upload " & ( multiple ? "files" : "file" );

        // Log the error for debugging
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
        // Set error state
        data.uploadFailed = true;

        // Create user-friendly error message
        data.errorMessage = "Failed to upload " & ( multiple ? "files" : "file" );

        // Log the error for debugging
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

CBWIRE 5.0 now provides access to the ColdBox event object (request context) directly in your templates. This allows you to use helpful methods like `event.buildLink()` for generating URLs within your CBWIRE components.

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
**Important:** Be cautious when using `event.getCollection()` (RC scope) or `event.getPrivateCollection()` (PRC scope) in CBWIRE templates. CBWIRE fires background requests to `/cbwire/update` on component re-renders, which may not have access to values set by interceptors or handlers that only run on your primary routes.

For example, if an interceptor sets a value in the PRC scope for your main routes but doesn't fire for `/cbwire/update`, that value won't be available during CBWIRE re-renders. If you need values from RC or PRC scopes, store them as data properties in your component's `onMount()` method to ensure they persist across re-renders.
{% endhint %}

### Single File BoxLang Components

CBWIRE 5.0 introduces support for single file BoxLang components using the `.bxm` file extension. This allows you to define both your component's template and logic in a single file, providing a more compact and streamlined development experience.

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

This approach keeps your component's template and logic together in one file, making it easier to understand and maintain smaller components.

See the [Single-file Components](../features/single-file-components.md) documentation for complete usage information.

### Custom Temporary Storage Path

CBWIRE 5.0 introduces the ability to configure a custom directory path for temporary file uploads using the new `storagePath` configuration setting. This enhancement is particularly valuable in distributed server environments where temporary files need to be shared across multiple front-end servers.

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

When configured, CBWIRE will use the specified directory instead of its default internal temporary storage location. This enables:

* Sharing temporary uploads across clustered servers using network-mounted storage
* Using specific disk volumes optimized for temporary file operations
* Implementing custom cleanup or monitoring policies on temporary file storage

See the [Configuration](../configuration.md#storagepath) and [File Uploads](../features/file-uploads.md) documentation for complete details.

### Improved Error Messaging for Empty Components

CBWIRE 5.0 provides clearer error messages when component templates are empty or missing HTML elements. Previously, you would encounter a cryptic error like "Cannot return first element of array; array is empty" when a component template had no HTML content. Now, CBWIRE displays a descriptive error message that clearly explains the issue and how to fix it.

Contributed by community member **David Moreno**.

**Previous Error (CBWIRE 4.x):**
```
Cannot return first element of array; array is empty
```

**New Error (CBWIRE 5.0):**
```
The HTML content of the wire component must contain at least one external element.
Wire component contains no HTML elements. It is empty.
```

### Reduced Dependency Injection Error Logging

Improved component scanning and dependency injection to eliminate unnecessary error logging for single file components. Previously, you might see multiple "ioc.Injector" error messages stating components were "not located in any declared scan location(s)" during component initialization. These harmless but noisy error messages have been resolved, resulting in cleaner application logs.

### External Module Location Support

CBWIRE 5.0 now supports loading wire components from external module locations defined in your ColdBox configuration. Previously, CBWIRE could only load components from the standard `/modules` directory. Now you can reference wire components from modules located in external directories configured via `modulesExternalLocation`.

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

You can now reference wires from external modules using the `@module` syntax:

```html
<div>
    #wire( name="MyComponent@externalModule" )#
</div>
```

This enhancement enables better module organization and supports multi-repository development workflows where modules are maintained separately from your main application.

## Breaking Changes

### Component Parameter Auto-Population

When a component defines an `onMount()` method, parameters passed via `wire()` are no longer automatically set to matching data properties. You must now explicitly assign parameters inside the `onMount()` method.

**Previous Behavior (CBWIRE 4.x):**
Parameters would automatically populate data properties even when `onMount()` was defined.

**New Behavior (CBWIRE 5.0):**
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

This change provides more explicit control over component initialization and makes the behavior more predictable.

See the [Components](../the-essentials/components.md#auto-populating-data-properties) documentation for complete details.

### Adobe ColdFusion 2018 Support Removed

CBWIRE 5.0 removes support for Adobe ColdFusion 2018, as it has reached end-of-life and is no longer officially supported by Adobe. CBWIRE 5.0 requires Adobe ColdFusion 2021 or later, Lucee 5.3 or later, or BoxLang.

**Supported Engines:**
- BoxLang
- Lucee 5.3+
- Adobe ColdFusion 2021+
- Adobe ColdFusion 2023+

If you are still using Adobe ColdFusion 2018, you should remain on CBWIRE 4.x or upgrade to a supported CFML engine before upgrading to CBWIRE 5.0.

## Bug Fixes

### File Upload Endpoint Configuration

Fixed file uploads to respect the `updateEndpoint` configuration setting. Previously, file uploads used a hardcoded `/cbwire/upload` path instead of dynamically generating the upload endpoint based on the configured update endpoint. File uploads now properly derive their endpoint from the `updateEndpoint` configuration, ensuring consistent routing behavior.

For example, if you configure a custom update endpoint like `/index.bxm/cbwire/update`, file uploads will now correctly use `/index.bxm/cbwire/upload` instead of the hardcoded path.

### File Upload Temporary Directory Path

Fixed incorrect file path generation in the `getUploadTempDirectory()` method. The method was generating malformed paths like `cbwiremodels/tmp` instead of `cbwire/models/tmp` due to missing path separation in the path concatenation. This has been corrected to ensure proper temporary directory path resolution for file uploads.

### Temporary Directory Race Condition

Fixed a race condition in higher-traffic environments that caused errors when multiple requests attempted to create the temporary directory simultaneously. The error "Can't create directory [/app/modules/cbwire/models/tmp], directory already exists" would occur when concurrent requests tried to create the same directory. Added proper locking mechanisms to prevent this race condition and ensure thread-safe directory creation.
