# File Uploads

Handle file uploads with automatic temporary storage, validation, and processing. CBWIRE automatically converts uploaded files into FileUpload objects with methods for accessing content, metadata, and preview URLs.

## Basic Usage

Create a photo upload component with save functionality:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/PhotoUpload.bx
class extends="cbwire.models.Component" {
    data = {
        "photo": "",
        "isUploading": false
    };

    function save() {
        if (data.photo != "") {
            // Move file to permanent location
            var storedPath = data.photo.store(expandPath("./uploads"));

            // Clean up temporary file
            data.photo.destroy();

            // Reset form
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
        "photo" = "",
        "isUploading" = false
    };

    function save() {
        if (data.photo != "") {
            // Move file to permanent location
            var storedPath = data.photo.store(expandPath("./uploads"));

            // Clean up temporary file
            data.photo.destroy();

            // Reset form
            data.photo = "";
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
        <div>
            <label for="photo">Select Photo:</label>
            <input type="file" 
                   id="photo" 
                   wire:model="photo" 
                   accept="image/*">
        </div>
        
        <!-- Loading indicator -->
        <div wire:loading wire:target="photo">
            Uploading photo...
        </div>
        
        <!-- Preview uploaded photo -->
        <bx:if photo != "" AND photo.isImage()>
            <div class="preview">
                <h3>Preview:</h3>
                <img src="#photo.getPreviewURL()#" 
                     alt="Uploaded photo" 
                     style="max-width: 200px;">
                <p>Size: #photo.getSize()# bytes</p>
            </div>
        </bx:if>
        
        <button type="submit" 
                wire:loading.attr="disabled">
            Save Photo
        </button>
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
        <div>
            <label for="photo">Select Photo:</label>
            <input type="file" 
                   id="photo" 
                   wire:model="photo" 
                   accept="image/*">
        </div>
        
        <!-- Loading indicator -->
        <div wire:loading wire:target="photo">
            Uploading photo...
        </div>
        
        <!-- Preview uploaded photo -->
        <cfif photo != "" AND photo.isImage()>
            <div class="preview">
                <h3>Preview:</h3>
                <img src="#photo.getPreviewURL()#" 
                     alt="Uploaded photo" 
                     style="max-width: 200px;">
                <p>Size: #photo.getSize()# bytes</p>
            </div>
        </cfif>
        
        <button type="submit" 
                wire:loading.attr="disabled">
            Save Photo
        </button>
    </form>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## How It Works

CBWIRE handles file uploads through a multi-step process:

1. **Request signed URL** - CBWIRE gets a temporary upload URL from the server
2. **Upload file** - JavaScript uploads the file to secure temporary storage
3. **Create FileUpload object** - The data property becomes a FileUpload instance
4. **Process file** - Use FileUpload methods to validate, manipulate, or store the file
5. **Store permanently** - Move files to permanent storage using the `store()` method

By default, CBWIRE stores uploaded files in the system's temporary directory (via `getTempDirectory()`) for enhanced security. This prevents unauthorized access to uploaded files before they're explicitly moved to permanent storage. You can configure a custom temporary storage path using the `uploadsStoragePath` setting. This is useful for distributed server environments where temporary files need to be shared across multiple servers. See the [Configuration](../configuration.md#uploadsstoragepath) documentation for details.

## FileUpload Methods

When a file is uploaded, CBWIRE creates a FileUpload object with the following methods:

### File Content

| Method           | Description                                     |
| ---------------- | ----------------------------------------------- |
| `get()`          | Returns the binary content of the uploaded file |
| `getBase64()`    | Returns base64 encoded string of the file       |
| `getBase64Src()` | Returns base64 data URL for use in `<img>` tags |

### File Information

| Method          | Description                                  |
| --------------- | -------------------------------------------- |
| `getSize()`     | Returns file size in bytes                   |
| `getMimeType()` | Returns the MIME type of the file            |
| `getMeta()`     | Returns all metadata about the uploaded file |
| `isImage()`     | Returns true if the file is an image         |

### File Locations

| Method                      | Description                             |
| --------------------------- | --------------------------------------- |
| `getTemporaryStoragePath()` | Returns the temporary file storage path |
| `getMetaPath()`             | Returns path to file metadata           |
| `getPreviewURL()`           | Returns URL for previewing images       |

### File Storage

| Method           | Description                                                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------ |
| `store( path )`  | Moves file from temporary to permanent storage. Path can be a directory or full file path. Creates destination directories automatically. Returns absolute path to stored file. |

### Cleanup

| Method      | Description                                             |
| ----------- | ------------------------------------------------------- |
| `destroy()` | Deletes temporary file and metadata (call after saving) |

{% hint style="warning" %}
Always call `destroy()` on FileUpload objects after saving to permanent storage to clean up temporary files.
{% endhint %}

{% hint style="info" %}
Image previews using `getPreviewURL()` only work with image file types. Use `isImage()` to check before displaying previews.
{% endhint %}

## Storing Files Permanently

The `store()` method moves uploaded files from temporary storage to a permanent location. This is the recommended approach for persisting uploaded files.

### Basic Usage

Pass a directory path to store the file with its original filename:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
function save() {
    if (data.photo != "") {
        // Store in directory with original filename
        var storedPath = data.photo.store(expandPath("./uploads"));

        writeLog("File stored at: #storedPath#");

        data.photo.destroy();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
function save() {
    if (data.photo != "") {
        // Store in directory with original filename
        var storedPath = data.photo.store(expandPath("./uploads"));

        writeLog("File stored at: #storedPath#");

        data.photo.destroy();
    }
}
```
{% endtab %}
{% endtabs %}

### Store with Custom Filename

Pass a full file path to store with a custom filename:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
function save() {
    if (data.document != "") {
        // Store with custom filename
        var customPath = expandPath("./uploads/#createUUID()#.pdf");
        var storedPath = data.document.store(customPath);

        // Save path to database
        saveDocumentPath(storedPath);

        data.document.destroy();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
function save() {
    if (data.document != "") {
        // Store with custom filename
        var customPath = expandPath("./uploads/#createUUID()#.pdf");
        var storedPath = data.document.store(customPath);

        // Save path to database
        saveDocumentPath(storedPath);

        data.document.destroy();
    }
}
```
{% endtab %}
{% endtabs %}

### Automatic Directory Creation

The `store()` method automatically creates destination directories if they don't exist:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
function save() {
    if (data.avatar != "") {
        // Directory will be created if it doesn't exist
        var userUploadDir = expandPath("./uploads/users/#data.userId#");
        var storedPath = data.avatar.store(userUploadDir);

        data.avatar.destroy();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
function save() {
    if (data.avatar != "") {
        // Directory will be created if it doesn't exist
        var userUploadDir = expandPath("./uploads/users/#data.userId#");
        var storedPath = data.avatar.store(userUploadDir);

        data.avatar.destroy();
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The `store()` method is more efficient than reading file content with `get()` and writing it manually. It moves the file directly using `fileMove()`, avoiding the overhead of reading and writing file content.
{% endhint %}

{% hint style="warning" %}
After calling `store()`, the FileUpload object's internal path is updated to the new location. You can continue to use FileUpload methods like `get()` or `getSize()` on the stored file. However, you should call `destroy()` after storing to clean up the metadata file in temporary storage.
{% endhint %}

## Loading States

Display upload progress using wire:loading directives:

```html
<input type="file" wire:model="document">

<div wire:loading wire:target="document">
    Uploading document...
</div>

<button type="submit" wire:loading.attr="disabled">
    Save Document
</button>
```

## Handling Upload Errors

The `onUploadError()` lifecycle hook is automatically called when a file upload fails with any HTTP response outside the 2xx range. This allows you to handle upload failures gracefully and provide feedback to users.

### Method Signature

```javascript
function onUploadError( property, errors, multiple )
```

| Parameter | Type    | Description                                                                                              |
| --------- | ------- | -------------------------------------------------------------------------------------------------------- |
| property  | string  | The name of the data property associated with the file input                                             |
| errors    | any     | The error response from the server. Will be null unless the HTTP status is 422, in which case it contains the response body |
| multiple  | boolean | Indicates whether multiple files were being uploaded (true) or a single file (false)                     |

### Usage Example

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
        data.errorMessage = "Failed to upload " & ( multiple ? "files" : "file" ) & " for " & property;

        // Log the error for debugging
        if ( !isNull( errors ) ) {
            writeLog( type="error", text="Upload error for #property#: #serializeJSON(errors)#" );
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
        data.errorMessage = "Failed to upload " & ( multiple ? "files" : "file" ) & " for " & property;

        // Log the error for debugging
        if ( !isNull( errors ) ) {
            writeLog( type="error", text="Upload error for #property#: #serializeJSON(errors)#" );
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
    <input type="file" wire:model="photo">

    <bx:if uploadFailed>
        <div class="alert alert-danger">
            #errorMessage#
        </div>
    </bx:if>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/photoUpload.cfm -->
<cfoutput>
<div>
    <input type="file" wire:model="photo">

    <cfif uploadFailed>
        <div class="alert alert-danger">
            #errorMessage#
        </div>
    </cfif>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

### When It's Called

The `onUploadError()` hook is triggered when:

- The upload server returns any HTTP status code outside the 2xx range (200-299)
- Network errors occur during upload
- The server is unreachable
- Any other upload failure scenario

### Error Information

The `errors` parameter provides different information based on the HTTP status:

- **422 (Unprocessable Entity)**: Contains the full response body (typically validation errors)
- **Other error codes**: Will be null

You can check the response status in your server logs or implement custom error handling based on your application's needs.

{% hint style="info" %}
The `onUploadError()` hook is optional. If not defined, upload errors will be handled silently by the JavaScript error callback. The hook is called AFTER the `upload:errored` event is dispatched to the JavaScript layer. All three parameters are always provided, though errors may be null.
{% endhint %}
