# What's New With 5.0

[Release Date TBD]

## Enhancements

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
