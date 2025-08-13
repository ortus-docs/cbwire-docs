# Configuration

CBWIRE provides extensive configuration options to customize its behavior for your application needs. You can override the default settings by adding configuration in your ColdBox application's **config/ColdBox.bx** for BoxLang or **config/ColdBox.cfc** for CFML.

All CBWIRE configuration is placed within the `moduleSettings` struct under the `cbwire` key. Here's a complete example showing all available configuration options with their default values:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
class {
    function configure() {
        moduleSettings = {
            "cbwire": {
                // Asset Management
                "autoInjectAssets": true,
                "moduleRootURL": "/modules/cbwire",
                
                // Component Configuration
                "wiresLocation": "wires",
                "trimStringValues": false,
                "throwOnMissingSetterMethod": false,
                
                // Request Handling
                "updateEndpoint": "/cbwire/updates",
                "maxUploadSeconds": 300, // 5 minutes
                
                // UI Features
                "showProgressBar": true,
                "progressBarColor": "#2299dd",
                
                // Security
                "csrfEnabled": false,
                "csrfStorage": "SessionStorage@cbstorages"
            }
        };
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
component {
    function configure() {
        moduleSettings = {
            "cbwire" = {
                // Asset Management
                "autoInjectAssets" = true,
                "moduleRootURL" = "/modules/cbwire",
                
                // Component Configuration
                "wiresLocation" = "wires",
                "trimStringValues" = false,
                "throwOnMissingSetterMethod" = false,
                
                // Request Handling
                "updateEndpoint" = "/cbwire/updates",
                "maxUploadSeconds" = 300, // 5 minutes
                
                // UI Features
                "showProgressBar" = true,
                "progressBarColor" = "##2299dd",
                
                // Security
                "csrfEnabled" = false,
                "csrfStorage" = "SessionStorage@cbstorages"
            }
        };
    }
}
```
{% endtab %}
{% endtabs %}

## Asset Management

### autoInjectAssets

Controls whether CBWIRE automatically includes its CSS and JavaScript assets in your pages. When enabled, you don't need to manually add `wireStyles()` in your layout's `<head>` or `wireScripts()` before the closing `</body>` tag.

**Default:** `true`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "autoInjectAssets": false
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "autoInjectAssets" = false
    }
};
```
{% endtab %}
{% endtabs %}

When disabled, you'll need to manually include the assets in your layout:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- In your layout's <head> -->
#wireStyles()#

<!-- Before closing </body> -->
#wireScripts()#
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- In your layout's <head> -->
#wireStyles()#

<!-- Before closing </body> -->
#wireScripts()#
```
{% endtab %}
{% endtabs %}

### moduleRootURL

Specifies the root URL path for CBWIRE module assets and endpoints. Useful when your application is deployed in a subdirectory or when using custom URL mappings.

**Default:** `/modules/cbwire`

{% hint style="info" %}
Setting `autoInjectAssets` to `false` is useful when you want more control over when and how CBWIRE assets are loaded, or when implementing custom asset optimization strategies.
{% endhint %}

## Component Configuration

### wiresLocation

Defines the directory where your CBWIRE components are stored, relative to your application root. This affects component auto-discovery and naming conventions.

**Default:** `wires`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "wiresLocation": "app/components"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "wiresLocation" = "app/components"
    }
};
```
{% endtab %}
{% endtabs %}

### trimStringValues

When enabled, automatically trims whitespace from string values in component data properties during updates. This is particularly useful for form inputs where users might accidentally include leading or trailing spaces.

**Default:** `false`

You can also enable this setting on individual components:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ContactForm.bx
class extends="cbwire.models.Component" {
    trimStringValues = true;
    
    data = {
        "name": "",
        "email": "",
        "message": ""
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ContactForm.cfc
component extends="cbwire.models.Component" {
    trimStringValues = true;
    
    data = {
        "name" = "",
        "email" = "",
        "message" = ""
    };
}
```
{% endtab %}
{% endtabs %}

### throwOnMissingSetterMethod

Controls whether CBWIRE throws an exception when trying to set a property that doesn't have a corresponding setter method. When disabled, missing setters are silently ignored.

**Default:** `false`

{% hint style="warning" %}
Changing the `wiresLocation` after components have been created will require updating your component references and possibly your routing configuration.
{% endhint %}

## Request Handling

### updateEndpoint

Sets the URI endpoint where CBWIRE processes component updates and actions. You may need to modify this if URL rewriting is disabled or if you're using custom routing.

**Default:** `/cbwire/updates`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "updateEndpoint": "/index.cfm/cbwire/updates"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "updateEndpoint" = "/index.cfm/cbwire/updates"
    }
};
```
{% endtab %}
{% endtabs %}

### maxUploadSeconds

Specifies the maximum time (in seconds) allowed for file upload operations to complete. This prevents long-running uploads from consuming server resources indefinitely.

**Default:** `300` (5 minutes)

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "maxUploadSeconds": 600 // 10 minutes
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "maxUploadSeconds" = 600 // 10 minutes
    }
};
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
When URL rewriting is disabled, remember to include `/index.cfm` in your `updateEndpoint` configuration.
{% endhint %}

## UI Features

### showProgressBar

Controls whether a progress bar appears at the top of the page during [wire:navigate](template-directives/wire-navigate.md) operations. The progress bar provides visual feedback during page transitions.

**Default:** `true`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "showProgressBar": false
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "showProgressBar" = false
    }
};
```
{% endtab %}
{% endtabs %}

### progressBarColor

Customizes the color of the progress bar displayed during [wire:navigate](template-directives/wire-navigate.md) operations. Accepts any valid CSS color value.

**Default:** `#2299dd`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "progressBarColor": "#ff6b35"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "progressBarColor" = "##ff6b35"
    }
};
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
The progress bar only appears when using `wire:navigate` for page transitions. Standard CBWIRE component updates don't trigger the progress bar.
{% endhint %}

## Security Configuration

### csrfEnabled

Enables Cross-Site Request Forgery (CSRF) protection for CBWIRE requests. When enabled, all component actions require a valid CSRF token.

**Default:** `false`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "csrfEnabled": true
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "csrfEnabled" = true
    }
};
```
{% endtab %}
{% endtabs %}

### csrfStorage

Specifies the WireBox mapping for the storage provider used to store CSRF tokens. The storage provider must implement the appropriate interface for session or cache storage.

**Default:** `SessionStorage@cbstorages`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
moduleSettings = {
    "cbwire": {
        "csrfStorage": "CacheStorage@cbstorages"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
moduleSettings = {
    "cbwire" = {
        "csrfStorage" = "CacheStorage@cbstorages"
    }
};
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
CSRF protection will be enabled by default starting in CBWIRE 5.0. It's recommended to enable and test CSRF protection in your applications now to ensure compatibility.
{% endhint %}

{% hint style="info" %}
All configuration options are optional. CBWIRE will use sensible defaults if no custom configuration is provided.
{% endhint %}