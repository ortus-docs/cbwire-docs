# Getting Started

## Overview

You can get started with CBWIRE with a few initial steps.

## Requirements

* BoxLang or CFML Server
  * BoxLang 1.0+
    * Currently requires [bx-compat-cfml](https://forgebox.io/view/bx-compat-cfml) and [bx-esapi](https://forgebox.io/view/bx-esapi) modules
  * Adobe ColdFusion 2021+
  * Lucee 5+
* ColdBox 6+

## Installation

Install [CommandBox](https://www.ortussolutions.com/products/commandbox).

Within the root of your project, run:

```bash
box install cbwire@4
```

If you want the latest bleeding edge, run:

```bash
box install cbwire@be
```

## BoxLang Setup

When using BoxLang with CBWIRE, you need to ensure that the required compatibility modules are installed. BoxLang requires the [bx-compat-cfml](https://forgebox.io/view/bx-compat-cfml) and [bx-esapi](https://forgebox.io/view/bx-esapi) modules to work with CBWIRE.

If you're launching your server using CommandBox, you can create a `server.json` file in your project root to automatically install these dependencies and configure your server:

```json
{
    "app":{
        "cfengine":"boxlang",
        "serverHomeDirectory":".engine/boxlang"
    },
    "web":{
        "rewrites":{
            "enable":"true"
        }
    },
    "JVM":{
        "javaVersion":"openjdk21_jdk"
    },
    "scripts":{
        "onServerInitialInstall":"install bx-compat-cfml,bx-esapi"
    }
}
```

This configuration will:
- Set BoxLang as the CFML engine
- Configure URL rewrites (recommended for ColdBox)
- Use OpenJDK 21 (recommended for BoxLang)
- Automatically install the required compatibility modules when the server starts for the first time

## Livewire Assets <a href="#layout-setup" id="layout-setup"></a>

For CBWIRE to work, your layout must include Livewire's CSS and JavaScript assets. CBWIRE 4 automatically adds these assets to your layout.

```html
<!doctype html>
<html>
<head>
	<!-- Livewire Styles -->
	<style >[wire\:loading][wire\:loading], [wire\:loading\.delay][wire\:loading\.delay], [wire\:loading\.inline-block][wire\:loading\.inline-block], [wire\:loading\.inline][wire\:loading\.inline], [wire\:loading\.block][wire\:loading\.block], [wire\:loading\.flex][wire\:loading\.flex], [wire\:loading\.table][wire\:loading\.table], [wire\:loading\.grid][wire\:loading\.grid], [wire\:loading\.inline-flex][wire\:loading\.inline-flex] {display: none;}[wire\:loading\.delay\.none][wire\:loading\.delay\.none], [wire\:loading\.delay\.shortest][wire\:loading\.delay\.shortest], [wire\:loading\.delay\.shorter][wire\:loading\.delay\.shorter], [wire\:loading\.delay\.short][wire\:loading\.delay\.short], [wire\:loading\.delay\.default][wire\:loading\.delay\.default], [wire\:loading\.delay\.long][wire\:loading\.delay\.long], [wire\:loading\.delay\.longer][wire\:loading\.delay\.longer], [wire\:loading\.delay\.longest][wire\:loading\.delay\.longest] {display: none;}[wire\:offline][wire\:offline] {display: none;}[wire\:dirty]:not(textarea):not(input):not(select) {display: none;}:root {--livewire-progress-bar-color: #2299dd;}[x-cloak] {display: none !important;}</style>
</head>
<body>
	<!-- Livewire SCRIPTS -->
	<script src="/modules/cbwire/includes/js/livewire.js?id=239a5c52" data-csrf="IwMh5yVu4bmvvh3krQjW50jdNHLPBQ6Ln7QLWEyc" data-update-uri="/cbwire/update" data-navigate-once="true"></script>
</body>
</html>
```

{% hint style="info" %}
Previous versions of CBWIRE required you to do this manually.
{% endhint %}

If you want to disable this functionality and manually include the CSS and JavaScript yourself, you can set the **autoInjectAssets** setting to **false** in your **ColdBox.bx or ColdBox.cfc**.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire" : {
        "autoInjectAssets": false
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```python
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" : {
        "autoInjectAssets": false
    }
};
```
{% endtab %}
{% endtabs %}

Then, you can manually add the CSS and JavaScript using the **wireStyles()** and **wireScripts()** methods in your layout file.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./layouts/Main.bxm --->
<bx:output>
<!DOCTYPE html>
<html>
    <head>
        <!--- CBWIRE CSS --->
        #wireStyles()#
    </head>
    <body>
        <!--- CBWIRE JS --->
        #wireScripts()#
    </body>
</html>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./layouts/Main.cfm --->
<cfoutput>
<!DOCTYPE html>
<html>
    <head>
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
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Your template must include Livewire's CSS and JavaScript; otherwise, CBWIRE will not work. If you are trying to use CBWIRE and it's not working as expected, this is the first place to check.
{% endhint %}
