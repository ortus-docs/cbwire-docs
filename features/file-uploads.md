---
description: >-
  Provide your users with file uploads and thumbnail previews without page
  refreshes.
---

# File Uploads

CBWIRE makes uploading and storing files easy.

Here's an example of a simple Wire that handles uploading a photo:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/UploadForm.bx
class extends="cbwire.models.Component" {
    data = {
        "photo": ""
    };
    
    function save() {
        // data.photo is now an instance of FileUpload ( see below )
        fileWrite( expandPath( "./somewhere.jpg" ), data.photo.get() );       
        data.photo.destroy();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/UploadForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "photo": ""
    };
    
    function save() {
        // data.photo is now an instance of FileUpload ( see below )
        fileWrite( expandPath( "./somewhere.jpg" ), data.photo.get() );       
        data.photo.destroy();
    }
}
```
{% endtab %}
{% endtabs %}

```html
<!--- ./wires/UploadForm.bxm|cfm --->
<form wire:submit.prevent="save">
    File: <input type="file" wire:model="photo">
    <div><button type="submit">Save</button></div>
</form>
```

Handling file inputs is no different than handling any other input type with CBWIRE. Add **wire:model** to the input tag and CBWIRE will take care of the rest.

Several things are happening under the hood to make file uploads work:

1. CBWIRE makes an initial request to the server to get a temporary "signed" upload URL.
2. Once the URL is received, JavaScript then does the actual "upload" to the signed URL, storing the upload in a temporary directory designated by CBWIRE and returning the new temporary file's unique hash ID.
3. Once the file is uploaded, and the unique hash ID is generated, CBWIRE makes a final request to the server, telling it to "set" the desired data property to the upload file as an instance of FileUpload ( see below ).
4. Now the data property (in this case, _photo_ ) is set to an instance of _FileUpload@cbwire_ and is ready to be stored or validated at any point.

## FileUpload Object

CBWIRE will automatically upload your files and set your associated data property to an instance of _FileUpload@cbwire_. Here are some of the methods you can use that are helpful.

| Method                    | Description                                                                                                                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| getComp()                 | Returns an instance of your current Wire.                                                                                                                                            |
| getParams()               | Returns any params that are passed in, including the data property name.                                                                                                             |
| get()                     | Returns the contents of the file uploaded.                                                                                                                                           |
| getBase64()               | Returns base64 encoded string of the file.                                                                                                                                           |
| getBase64Src()            | Returns a base64 src string that can be used with \<img tag>. It is recommended to use this carefully because with larger objects, this can slow down CBWIRE response times.         |
| getTemporaryStoragePath() | Returns the temporary storage path where the file is stored by CBWIRE.                                                                                                               |
| getMetaPath()             | Returns the file path to meta information of the uploaded file. You can find additional details here like the name of the file when it was uploaded, the client file extension, etc. |
| getMeta()                 | Returns all captured meta information on the file upload.                                                                                                                            |
| getSize()                 | Returns the size of the file upload.                                                                                                                                                 |
| getMimeType()             | Returns the mime type of the file upload.                                                                                                                                            |
| isImage()                 | Returns true if this is an image.                                                                                                                                                    |
| getPreviewURL()           | Provides a URL to preview the file uploaded. It only works with image uploads.                                                                                                       |
| destroy()                 | Deletes the temporary storage and metadata of the file upload. It's essential to use this after storing the file upload in permanent storage.                                        |

## Loading Indicators

You can display a loading indicator scoped to the file input during upload like so:

```html
<input type="file" wire:model="photo">
 
<div wire:loading wire:target="photo">Uploading...</div>
```

The "Uploading..." message will be shown as the file is uploading and then hidden when the upload is finished.

This works with the entire [Loading States API](broken-reference).
