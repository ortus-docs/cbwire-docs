# Configuration

You can alter cbwire's behavior by overriding settings in your `config/ColdBox.cfc` file.

```javascript
// File: ./config/ColdBox.cfc

component{
    function configure() {
        moduleSettings = {
            cbwire = {
                "throwOnMissingSetterMethod" : false,
                "componentLocation": "wires"
            }
        };
     }
}

```

{% hint style="info" %}
Overriding the module settings is optional. All settings come with a default that is used if no overrides are provided. See [Overriding Module Settings](https://coldbox.ortusbooks.com/hmvc/modules/moduleconfig/module-settings#overriding-module-settings) in the ColdBox documentation for additional reference.
{% endhint %}

### throwOnMissingSetter

Set as `true` to throw a `WireSetterNotFound` exception if the incoming wire request tries to update a  property without a setter on our component. Otherwise, missing setters are ignored. **Defaults to false**.

## componentLocation

The relative folder path where the cbwire components are stored. **Defaults to `wires`**.
