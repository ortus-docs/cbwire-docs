# What's New With 4.1

09/29/2024

## New Features

### Assets Management

CBWIRE 4.1 introduces `<cbwire:script>` and `<cbwire:assets>` functionality for better control over component-specific assets. These assets are automatically tracked throughout the request and appended to the `<head>` tag during page load.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/dashboard.bxm -->
<bx:output>
<cbwire:script>
    console.log('Dashboard component loaded');
    // Component-specific JavaScript
</cbwire:script>

<cbwire:assets>
    <link rel="stylesheet" href="/css/dashboard.css">
    <script src="/js/chart-library.js"></script>
</cbwire:assets>

<div>
    <h1>Dashboard</h1>
    <!-- Component content -->
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/dashboard.cfm -->
<cfoutput>
<cbwire:script>
    console.log('Dashboard component loaded');
    // Component-specific JavaScript
</cbwire:script>

<cbwire:assets>
    <link rel="stylesheet" href="/css/dashboard.css">
    <script src="/js/chart-library.js"></script>
</cbwire:assets>

<div>
    <h1>Dashboard</h1>
    <!-- Component content -->
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

### Locked Data Properties

Protect sensitive data properties from client-side modifications by defining a `locked` variable in your component. This prevents certain properties from being updated via `wire:model` or other client interactions.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserProfile.bx
class extends="cbwire.models.Component" {
    // Lock single property
    locked = "userId";
    
    // Or lock multiple properties
    locked = ["userId", "role", "permissions"];
    
    data = {
        "userId": 123,
        "name": "John Doe",
        "email": "john@example.com",
        "role": "admin",
        "permissions": ["read", "write"]
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    // Lock single property
    locked = "userId";
    
    // Or lock multiple properties
    locked = ["userId", "role", "permissions"];
    
    data = {
        "userId" = 123,
        "name" = "John Doe",
        "email" = "john@example.com",
        "role" = "admin",
        "permissions" = ["read", "write"]
    };
}
```
{% endtab %}
{% endtabs %}

```html
<!-- Template can bind to name/email but userId/role are protected -->
<input wire:model="name" placeholder="Name">
<input wire:model="email" placeholder="Email">
<!-- These would throw "Locked properties cannot be updated" if attempted -->
```

### Module Root URL Configuration

Configure custom module root URLs for better control over CBWIRE routing in complex applications.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        "moduleRootURL": "/custom/cbwire/path"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "moduleRootURL" = "/custom/cbwire/path"
    }
};
```
{% endtab %}
{% endtabs %}

### File Upload Enhancement

New `getTemporaryStoragePath()` method in FileUpload components provides access to temporary file storage locations.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/FileManager.bx
class extends="cbwire.models.Component" {
    function processUpload() {
        if (data.upload) {
            var tempPath = data.upload.getTemporaryStoragePath();
            // Process file at temporary location
        }
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/FileManager.cfc
component extends="cbwire.models.Component" {
    function processUpload() {
        if (structKeyExists(data, "upload")) {
            var tempPath = data.upload.getTemporaryStoragePath();
            // Process file at temporary location
        }
    }
}
```
{% endtab %}
{% endtabs %}

### Progress Bar Customization

Enhanced progress bar configuration with proper color setting and disable functionality.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        "showProgressBar": false,           // Disable completely
        "progressBarColor": "##00ff00"      // Custom color (fixed)
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "showProgressBar" = false,          // Disable completely
        "progressBarColor" = "##00ff00"     // Custom color (fixed)
    }
};
```
{% endtab %}
{% endtabs %}

### String Trimming

Automatically trim whitespace from string values in form inputs.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// Global setting
moduleSettings = {
    "cbwire": {
        "trimStringValues": true
    }
};

// Or per component
class extends="cbwire.models.Component" {
    function shouldTrimStringValues() {
        return true;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Global setting
moduleSettings = {
    "cbwire" = {
        "trimStringValues" = true
    }
};

// Or per component
component extends="cbwire.models.Component" {
    function shouldTrimStringValues() {
        return true;
    }
}
```
{% endtab %}
{% endtabs %}

### External Module Support

Load CBWIRE components from external module locations for better code organization.

### Update Endpoint Configuration

Customize the CBWIRE update endpoint for advanced routing scenarios.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        "updateEndpoint": "/custom/cbwire/update"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "updateEndpoint" = "/custom/cbwire/update"
    }
};
```
{% endtab %}
{% endtabs %}

## Enhancements

### Asset Rendering Optimization

- Assets are now tracked throughout the request and only rendered once
- Component-specific assets are automatically injected into the `<head>` tag
- Prevents duplicate asset loading for better performance

### Performance Improvements

- CBWIRE rendering speed improvements
- Enhanced child/nested component tracking
- Optimized asset management and injection

## Bug Fixes

### Component Lifecycle

- **Fixed onRender() method**: Restored missing `onRender()` method from previous versions
- **Fixed onUpdate() behavior**: Resolved issue where `onUpdate()` was called when no updates occurred

### Component Management

- **Child/nested component tracking**: Improved tracking of child components in Livewire
- **Component isolation**: Better handling of component isolation and lazy loading

### Asset Management

- **Duplicate asset prevention**: Assets that have already been rendered are not returned again
- **Asset injection**: Component assets are properly tracked and injected into page head

### External Module Support

- **Module loading**: Fixed issues loading wires from external module locations
- **Path resolution**: Improved component path resolution for complex module structures