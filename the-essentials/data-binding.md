# Data Binding

## Overview

You can bind your [data properties](properties.md) to input elements within your [template](templates.md) using [**wire:model**](../template-directives/wire-model.md).

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/MyComponent.bx
class extends="cbwire.models.Component" {
    
    // Data properties
    data = {
        "name": ""
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/MyComponent.cfc
component extends="cbwire.models.Component" {
    
    // Data properties
    data = {
        "name": ""
    };
}
```
{% endtab %}
{% endtabs %}

```html
<!--- ./wires/MyComponent.bxm|cfm --->
<div>
    <form>
        <input wire:model.live="name" type="text">
        Name updated at #now()#
    </form>
</div>
```

## Resources

{% hint style="success" %}
See the [wire:model](../template-directives/wire-model.md) page for complete documentation.
{% endhint %}

{% hint style="info" %}
When [data properties](properties.md) are updated using **wire:model**, CBWIRE has several methods you can hook into such as **onUpdate** and **onHyrdate** ( See [Lifecycle Methods](lifecycle-events.md) ).
{% endhint %}
