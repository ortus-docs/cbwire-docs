# Getting Started

CBWIRE brings reactive UI capabilities to your BoxLang and CFML applications, making it easy to build dynamic interfaces without complex JavaScript frameworks. This guide will help you set up CBWIRE and create your first reactive component.

## System Requirements

Before installing CBWIRE, ensure your system meets these requirements:

### BoxLang Requirements

* **BoxLang 1.0+**
* **CBWIRE 4.1+** (BoxLang support was introduced in version 4.1)
* **OpenJDK 21** (required for BoxLang)
* **ColdBox 6+**
* **Required modules:**
  * [bx-compat-cfml](https://forgebox.io/view/bx-compat-cfml) - CFML compatibility layer
  * [bx-esapi](https://forgebox.io/view/bx-esapi) - Security encoding functions

### CFML Requirements

* **Adobe ColdFusion 2021+** or **Lucee 5+**
* **ColdBox 6+**

## Installation

CBWIRE is distributed through ForgeBox and can be installed using CommandBox.

### Install CommandBox

If you don't have CommandBox installed, download it from [ortussolutions.com/products/commandbox](https://www.ortussolutions.com/products/commandbox).

### Install CBWIRE

Navigate to your ColdBox application root and run:

```bash
box install cbwire@4
```

For the latest development version:

```bash
box install cbwire@be
```

## BoxLang Setup

BoxLang applications require additional configuration to work with CBWIRE. The easiest way to set this up is by creating a `server.json` file in your project root:

```json
{
    "app": {
        "cfengine": "boxlang",
        "serverHomeDirectory": ".engine/boxlang"
    },
    "web": {
        "rewrites": {
            "enable": "true"
        }
    },
    "JVM": {
        "javaVersion": "openjdk21_jdk"
    },
    "scripts": {
        "onServerInitialInstall": "install bx-compat-cfml,bx-esapi"
    }
}
```

This configuration automatically:

* Sets BoxLang as the CFML engine
* Enables URL rewrites (recommended for ColdBox applications)
* Uses OpenJDK 21 (required for BoxLang)
* Installs required compatibility modules on first server start

Start your server with:

```bash
box start
```

{% hint style="info" %}
The compatibility modules are only required for BoxLang applications. CFML applications using Adobe ColdFusion or Lucee don't need these additional modules.
{% endhint %}

## Asset Integration

CBWIRE requires CSS and JavaScript assets to function properly. By default, CBWIRE 4 automatically injects these assets into your application's layout, so no manual configuration is needed.

### Automatic Asset Injection (Default)

When automatic asset injection is enabled (default), CBWIRE will automatically include the necessary CSS in your page's `<head>` and JavaScript before the closing `</body>` tag. Your layout will automatically include content similar to this:

```html
<!doctype html>
<html>
<head>
    <!-- CBWIRE CSS (automatically injected) -->
    <style>[wire\:loading] { display: none; } /* ... more styles ... */</style>
</head>
<body>
    <!-- Your content -->
    
    <!-- CBWIRE JavaScript (automatically injected) -->
    <script src="/modules/cbwire/includes/js/livewire.js" data-csrf="..." data-update-uri="/cbwire/update"></script>
</body>
</html>
```

### Manual Asset Management

If you prefer to control when and how assets are loaded, you can disable automatic injection:

```javascript
// config/ColdBox.bx (BoxLang) or config/ColdBox.cfc (CFML)
moduleSettings = {
    "cbwire": {
        "autoInjectAssets": false
    }
};
```

Then manually include the assets in your layout:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- layouts/Main.bxm -->
<bx:output>
<!DOCTYPE html>
<html>
    <head>
        <!-- CBWIRE CSS -->
        #wireStyles()#
    </head>
    <body>
        <!-- Your application content -->
        
        <!-- CBWIRE JavaScript -->
        #wireScripts()#
    </body>
</html>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- layouts/Main.cfm -->
<cfoutput>
<!DOCTYPE html>
<html>
    <head>
        <!-- CBWIRE CSS -->
        #wireStyles()#
    </head>
    <body>
        <!-- Your application content -->
        
        <!-- CBWIRE JavaScript -->
        #wireScripts()#
    </body>
</html>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Next Steps

Congratulations! You've successfully set up CBWIRE. Here are some recommended next steps:

* **Learn the Essentials**: Explore [Components](the-essentials/components.md), [Templates](the-essentials/templates.md), and [Data Binding](the-essentials/properties.md)
* **Explore Actions**: Understand how [Actions](the-essentials/actions.md) work and handle user interactions
* **Master Template Directives**: Learn about [wire:click](template-directives/wire-click.md), [wire:model](template-directives/wire-model.md), and other directives
* **Advanced Features**: Discover [Events](the-essentials/events.md), [File Uploads](features/file-uploads.md), and [Alpine.js Integration](https://github.com/ortus-docs/cbwire-docs/blob/v4.x/features/alpine-js.md)

## Troubleshooting

If you encounter issues:

1. **Verify assets are loaded**: Check your browser's developer tools to ensure CBWIRE CSS and JavaScript are present
2. **Check server logs**: Look for any CBWIRE-related errors in your application logs
3. **Validate requirements**: Ensure all system requirements are met, especially for BoxLang applications
4. **Review configuration**: Double-check your ColdBox configuration if using custom settings

{% hint style="warning" %}
CBWIRE requires its CSS and JavaScript assets to function. If components aren't responding to interactions, verify that assets are properly loaded in your browser's developer tools.
{% endhint %}

{% hint style="info" %}
Previous versions of CBWIRE required manual asset inclusion. CBWIRE 4 automatically handles this by default, making setup much simpler.
{% endhint %}
