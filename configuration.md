---
description: Change the internal behavior of CBWIRE as needed.
---

# Configuration

## ColdBox.cfc

You can alter any default behavior by overriding settings in your **config/ColdBox.cfc** file.

```javascript
// File: ./config/ColdBox.cfc
component{
    function configure() {
        moduleSettings = {
            cbwire = {
                "autoInjectAssets": false,
                "cacheSingleFileComponents": false,
                "enableTurbo": false,
                "maxUploadSeconds": 5 * 60, // 5 minutes
                "throwOnMissingSetterMethod" : false,
                "trimStringValues": false,
                "wiresLocation": "wires"
            }
        };
     }
}

```

{% hint style="info" %}
Overriding the module settings is optional.
{% endhint %}

### autoInjectAssets

Set as true to automatically include CBWIRE's CSS and JavaScript assets. This removes the need to manually add references to wireStyles() in your \<head> and wireScripts() at the end of \</body> in your ColdBox layout file. In the next major release, this will default to true. **Defaults to false.**

### cacheSingleFileComponents

Set as true to enable caching of single-file components, meaning Components that are placed in a single .cfm file instead of separating them into a .cfc and .cfm files. CBWIRE requires additional processing to render single-file components. Enabling this setting will increase rendering performance. When enabled, you will need to reinit ColdBox when making changes to the component. We recommend enabling this in production but leaving it set to false when developing your app. **Defaults to false**.

### enableTurbo

Set as true to enable [Turbo](integrations/spas-with-turbo.md), which causes any clicked links or form submissions to be performed in the background via AJAX and updates the DOM without reloading the page. Great when developing single-page applications. **Defaults to false.**

### maxUploadSeconds

The maximum amount of time allowed for uploads to complete.

### throwOnMissingSetter

Set as true to throw a WireSetterNotFound exception if the incoming wire request tries to update a property without a setter on our component. Otherwise, missing setters are ignored. **Defaults to false**.

### trimStringValues

When set to true, any data properties that contain strings will be automatically trimmed. Great for form inputs. **Defaults to false**.

### wiresLocation

The relative folder path where components are stored. **Defaults to 'wires'**.

