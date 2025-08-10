---
description: Every journey starts somewhere.
---

# Getting Started

## Requirements

* Adobe ColdFusion 2018+ or Lucee 5+
* ColdBox 6+

## Installation

Install [CommandBox](https://www.ortussolutions.com/products/commandbox).

Within the root of your project, run:

```bash
box install cbwire@3
```

If you want the latest bleeding edge, run:

```bash
box install cbwire@be
```

## Layout Setup

CBWIRE requires some lightweight CSS and JavaScript to be added to your ColdBox layout.

You can have CBWIRE do this for you by enabling the **autoInjectAssets** [Configuration](configuration.md) property in your config/ColdBox.cfc file.

```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" : {
        "autoInjectAssets": true
    }
};
```

You can also manually add the references by calling **wireStyles()** and **wireScripts()** in your layout file.

```html
<!--- ./layouts/Main.cfm --->
<cfoutput>
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>My Page</title>
        <!--- CBWIRE CSS --->
        #wireStyles()#
    </head>
    <body>
        <!--- CBWIRE JS --->
        #wireScripts()#
    </body>
</html>
</cfoutput>
```

{% hint style="warning" %}
CBWIRE will not work if you do not add the wireStyles() and wireScripts() references to your layout or enable autoInjectAssets.
{% endhint %}

## Next Steps

You are ready to go. Now head to the [Making Components](essentials/components.md) section to learn how to create and use CBWIRE components.

We also have numerous [Examples](examples/) you can review to get up and running quickly.
