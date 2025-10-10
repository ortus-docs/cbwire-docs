# Lifecycle Methods

CBWIRE provides lifecycle methods you can hook into to update and render your [components](components.md).

## Order Of Operations

The lifecycle methods are executed in the following order when a component is initially loaded:

1. onMount()
2. onRender()

Lifecycle methods are executed in this order for subsequent AJAX requests.

1. onHydrate\[DataProperty]\()
2. onHydrate()
3. onUpdate\[DataProperty]\()
4. onUpdate()
5. Fire actions
6. onRender()

{% tabs %}
{% tab title="BoxLang" %}
```javascript
class extends="cbwire.models.Component" {
    data = {
        "someValue: ""
    };
    
    function onMount( event, rc, prc, params ){
        data.someValue = params.someValue;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
component extends="cbwire.models.Component" {
    data = {
        "someValue: ""
    };
    
    function onMount( event, rc, prc, params ){
        data.someValue = params.someValue;
    }
}
```
{% endtab %}
{% endtabs %}

## Methods

### onMount

It runs only once when a component is initially wired. This can inject data values into your component via **params** you pass in when calling **wire()**, or pulling in values from the RC or PRC scopes.

```javascript
function onMount( event, rc, prc, params ){
    data.someValue = params.someValue;
}
```

{% hint style="warning" %}
**onMount()** only fires when the component is initially rendered and does not fire on subsequent requests when your component re-renders. This can cause issues referencing things such as the RC or PRC scope. If you pass in values with the RC or PRC scope, you must store them as data properties to ensure they are available to your component in subsequent requests.
{% endhint %}

### onRender

It runs on all requests before rendering your component. This gives you more control if needed when rendering. There is also a **renderIt()** alias, which does the same.

```javascript
function onRender() {
    return "<div>direct html</div>;
    // return template( _getViewPath() ); // CBWIRE's default method
    // return template( "some.custom.path" );
}
```

### onHydrate

Runs on subsequent requests after a component is hydrated but before [computed properties](computed-properties.md) are rendered, before a [data property](properties.md) is updated or [action](actions.md) is performed, or before the component is rendered.

```javascript
function onHydrate( data ) {
    // Note that computed properties have not yet rendered
    data.hydrated = true;
}
```

### onHydrate\[ Property ]

Runs on subsequent requests after a specific data property is hydrated but before computed properties are rendered, before an action is performed, or before the component is rendered.

```javascript
data = {
    "count": 1
};
function onHydrateCount( data ) {
    // Note that computed properties have not yet rendered
    data.count += 1;
}  
```

### onUpdate

Runs on subsequent requests after any [data property](properties.md) is updated using **wire:model** or **$set**.

```javascript
function onUpdate( newValues, oldValues ) {
    // ....
}
```

{% hint style="info" %}
**onUpdate()** will only fire if the incoming request updates a single data property, such as when using **wire:model**.
{% endhint %}

### onUpdate\[ Property ]

Runs on subsequent requests after a [data property](properties.md) is updated using **wire:model** or **$set**. It only runs when the targeted data property is updated.

```javascript
data = {
    "count": 1
};

function onUpdateCount( newValue, oldValue ) {
    if ( newValue >= 100 ) {
        // Reset back to 1
        data.count = 1;
    }
}
```

### onUploadError

Runs automatically when a file upload encounters an error (any HTTP response with a non-2xx status code). This allows you to handle upload failures gracefully in your CBWIRE components.

```javascript
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
```

| Parameter | Type    | Description                                                                                              |
| --------- | ------- | -------------------------------------------------------------------------------------------------------- |
| property  | string  | The name of the data property associated with the file input                                             |
| errors    | any     | The error response from the server. Will be null unless the HTTP status is 422, in which case it contains the response body |
| multiple  | boolean | Indicates whether multiple files were being uploaded (true) or a single file (false)                     |

{% hint style="info" %}
See the [File Uploads](../features/file-uploads.md#handling-upload-errors) documentation for complete upload error handling information.
{% endhint %}
