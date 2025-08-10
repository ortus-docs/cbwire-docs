---
description: Alter CBWIRE's behavior with these nifty configuration options.
---

# Configuration

## ColdBox.cfc

You can alter any default behavior by overriding settings in your `config/ColdBox.cfc` file.

```javascript
// File: ./config/ColdBox.cfc
component{
    function configure() {
        moduleSettings = {
            cbwire = {
                "enableTurbo": false,
                "maxUploadSeconds": 5 * 60, // 5 minutes
                "throwOnMissingSetterMethod" : false,
                "trimStringValues": false,
                "wiresLocation": "myWires",
                "useComputedPropertiesProxy": false
            }
        };
     }
}

```

{% hint style="info" %}
Overriding the module settings is optional. All settings come with a default that is used if no overrides are provided. See [Overriding Module Settings](https://coldbox.ortusbooks.com/hmvc/modules/moduleconfig/module-settings#overriding-module-settings) in the ColdBox documentation for additional reference.
{% endhint %}

### enableTurbo

Set as true to enable [Turbo](../integrations/turbo.md), which causes any clicked links or form submissions to be performed in the background via AJAX and updates the DOM without reloading the page. Great when developing single-page, VueJS-like applications. **Defaults to false.**

### maxUploadSeconds

The maximum amount of time allowed for uploads to complete.

### throwOnMissingSetter

Set as true to throw a WireSetterNotFound exception if the incoming wire request tries to update a property without a setter on our [Wire](creating-components.md). Otherwise, missing setters are ignored. **Defaults to false**.

### trimStringValues

When set to true, any data properties that contain strings will be automatically trimmed. Great for form inputs. **Defaults to false**.

### wiresLocation

The relative folder path where [Wires](creating-components.md) are stored. **Defaults to 'wires'**.

### useComputedPropertiesProxy

When set to true, Computed Properties will be proxied. See [Computed Properties (Proxied)](computed-properties/computed-properties-proxied.md).

