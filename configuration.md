# Configuration

You can alter CBWIRE and Livewire's default behavior by overriding settings in your **config/ColdBox.cfc** file.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./config/ColdBox.bx
class {
    function configure() {
        moduleSettings = {
            "cbwire" : {
                "autoInjectAssets": false,
                "maxUploadSeconds": 5 * 60, // 5 minutes
                "throwOnMissingSetterMethod" : false,
                "trimStringValues": false,
                "wiresLocation": "wires",
                "updateEndpoint": "/cbwire/updates",
                "showProgressBar": true,
                "progressBarColor": "##2299dd",
                "csrfEnabled": false,
                "csrfStorage": "SessionStorage@cbstorages",
                "moduleRootURL": "/modules/cbwire"
            }
        };
     }
}

```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./config/ColdBox.cfc
component {
    function configure() {
        moduleSettings = {
            "cbwire" = {
                "autoInjectAssets": false,
                "maxUploadSeconds": 5 * 60, // 5 minutes
                "throwOnMissingSetterMethod" : false,
                "trimStringValues": false,
                "wiresLocation": "wires",
                "updateEndpoint": "/cbwire/updates",
                "showProgressBar": true,
                "progressBarColor": "##2299dd",
                "csrfEnabled": false,
                "csrfStorage": "SessionStorage@cbstorages",
                "moduleRootURL": "/modules/cbwire"
            }
        };
     }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Overriding the module settings is optional.
{% endhint %}

## autoInjectAssets

Automatically include Livewire's CSS and JavaScript assets. This removes the need to manually add references to **wireStyles()** in your **\<head>** and **wireScripts()** at the end of **\</body>** in your ColdBox layout file. **Defaults to true.**

## maxUploadSeconds

The maximum amount of time allowed for uploads to complete.

## trimStringValues

When set to true, any [data properties](the-essentials/properties.md) that contain strings will be automatically trimmed on updates. Great for form inputs. **Defaults to false.**

You can enable it directly on your [components](the-essentials/components.md) if you don't want to set this globally.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
class extends="cbwire.models.Component" {
    trimStringValues = true;
    data = {
        "name": ""  
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
component extends="cbwire.models.Component" {
    trimStringValues = true;
    data = {
        "name": ""  
    };
}
```
{% endtab %}
{% endtabs %}

## wiresLocation

The relative folder path where [components](the-essentials/components.md) are stored. **Defaults to 'wires'.**

## updateEndpoint

Sets the URI endpoint where CBWIRE posts its updates to the server. You might need to change this if you do not have URL rewriting enabled.

```json
{
    "updateEndpoint": "/index.cfm/cbwire/update"
}
```

## showProgressBar

When set to true, it displays a progress bar at the top of the page using [wire:navigate](template-directives/wire-navigate.md) to load pages. **Defaults to true.** Set to **false** to disable the progress bar altogether.

## progressBarColor

Use to adjust the progress bar color when using [wire:navigate](template-directives/wire-navigate.md).

## csrfEnabled

Determines if CSRF token protection is enabled or not. **Defaults to false. This will be changed to 'true' in CBWIRE 5.**

## csrfStorage

CBWIRE uses CSRF tokens to protect incoming requests from bad actors. You can set the wirebox mapping to determine what storage provider is used when storing the CSRF tokens. **Defaults to SessionStorage@cbstorages.**

### **moduleRootURL**

Override to change the URL root path to CBWIRE.
