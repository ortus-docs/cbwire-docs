---
description: >-
  Remove full-page reloads from your applications and create single-page
  applications with ease using Turbo.
---

# Turbo

Turbo is an open-source library that accelerates links and form submissions by negating the need for full page reloads. With Turbo, you get all the following included:

* Links and form submissions are performed in the background using AJAX instead of performing full page reloads
* Browse history management, so clicking the back and forward buttons in the browser work as expected
* Internal caching that improves perceived performance by showing temporary previews during network requests and while the page is updated
* [Built-in progress bar](turbo.md#displaying-progress)
* [Pre-load Links](turbo.md#undefined)
* And much more!

CBWIRE and Turbo together allow you to quickly build single-page applications in record time.

{% hint style="success" %}
For additional information on how Turbo can be used and configured, please see [https://turbo.hotwired.dev/](https://turbo.hotwired.dev/)
{% endhint %}

## Enabling Turbo

You can enable Turbo with the **enableTurbo** [configuration setting](../essentials/configuration.md), which automatically wires up everything needed to start using Turbo. Once enabled, links and form submissions will automatically be performed in the background via AJAX instead of performing full page reloads.&#x20;

You can alternatively perform a [manual installation](turbo.md#manual-installation) of Turbo instead.

```javascript
// File: ./config/ColdBox.cfc
component{
    function configure() {
        moduleSettings = {
            "cbwire" = {
                "enableTurbo": true
            }
        };
     }
}

```

## Manual Installation

{% hint style="info" %}
Use the manual installation when you want more control over where Turbo is included within your HTML.
{% endhint %}

### npm

You can install Turbo by running the following `npm` command in the root of your project.

```bash
npm i @hotwired/turbo
```

Then you can require or import Turbo.

```javascript
import * as Turbo from "@hotwired/turbo"
```

### Skypack

To avoid manual installation, there is also a [Skypack](https://www.skypack.dev/view/@hotwired/turbo) available which you can add to the \<head>\</head> of your layout.

```html
<script type="module">
    import hotwiredTurbo from 'https://cdn.skypack.dev/@hotwired/turbo';
</script>
```

### Livewire/Turbo Plugin

For Turbo to work properly with Livewire (and therefore CBWIRE), you will also need to include the Turbo plugin below your `wireScripts()` call in your layout.

```javascript
    #wireScripts()#
    <script src="https://cdn.jsdelivr.net/gh/livewire/turbolinks@v0.1.x/dist/livewire-turbolinks.js" data-turbolinks-eval="false" data-turbo-eval="false"></script>
</body>
```

{% hint style="info" %}
Note: You MUST have either the `data-turbolinks-eval="false"` or `data-turbo-eval="false"` attributes added to the script tag (having both won't hurt).
{% endhint %}

## Displaying Progress&#x20;

During Turbo navigation, the browser will not display its native progress indicator. Turbo installs a CSS-based progress bar to provide feedback while issuing a request.

The progress bar is enabled by default. It appears automatically for any page that takes longer than 500ms to load.

The progress bar is a `<div>` element with the class name `turbo-progress-bar`. Its default styles appear first in the document and can be overridden by rules that come later.

For example, the following CSS will result in a thick green progress bar:

```css
.turbo-progress-bar {
    height: 5px;
    background-color: green;
}
```

In your layout file, you can include the progress bar like so:

```html
<!-- ./layouts/Main.cfm -->
<cfoutput>
<html>
    <head>
        <title>Turbo Page</title>
        #wireStyles()#
    </head>
    <body>
        <div class="turbo-progress-bar"></div>
        <div class="mainContent">
            Some content here
        </div>
        #wireScripts()#
    </body>
</html>
</cfoutput>
```

## Preload Links into Turbo's Cache

Preload links into Turbo Drive’s cache using **data-turbo-preload**.

```html
<a href="/" data-turbo-preload>Home</a>
```

This will make page transitions feel lightning fast by providing a preview of a page even before the first visit. Use it to preload the most important pages in your application.&#x20;

{% hint style="warning" %}
Avoid over usage, as it will lead to loading content that is not needed.
{% endhint %}
