# wire:ignore

The `wire:ignore` directive tells CBWIRE to skip updating specific parts of your template during re-renders. This is essential when integrating third-party JavaScript libraries, Alpine.js components, or preserving content that should remain static across component updates.

## Basic Usage

This dashboard component demonstrates `wire:ignore` protecting third-party content and preserving static elements:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Dashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "refreshCount": 0,
        "loadTime": now()
    };

    function refresh() {
        data.refreshCount++;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Dashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "refreshCount" = 0,
        "loadTime" = now()
    };

    function refresh() {
        data.refreshCount++;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/dashboard.bxm -->
<bx:output>
<div>
    <h1>Dashboard (Refreshed #refreshCount# times)</h1>
    
    <!-- This content never updates -->
    <div wire:ignore>
        <p>Component first loaded: #dateFormat(loadTime, "yyyy-mm-dd")# #timeFormat(loadTime, "HH:mm:ss")#</p>
        <div id="third-party-chart"></div>
    </div>
    
    <!-- This Alpine.js component preserves its state -->
    <div wire:ignore.self x-data="{ count: 0 }" class="counter">
        <button @click="count++">Alpine Counter: <span x-text="count"></span></button>
        <p>Current time: #timeFormat(now(), "HH:mm:ss")#</p>
    </div>
    
    <button wire:click="refresh">Refresh Component</button>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/dashboard.cfm -->
<cfoutput>
<div>
    <h1>Dashboard (Refreshed #refreshCount# times)</h1>
    
    <!-- This content never updates -->
    <div wire:ignore>
        <p>Component first loaded: #dateFormat(loadTime, "yyyy-mm-dd")# #timeFormat(loadTime, "HH:mm:ss")#</p>
        <div id="third-party-chart"></div>
    </div>
    
    <!-- This Alpine.js component preserves its state -->
    <div wire:ignore.self x-data="{ count: 0 }" class="counter">
        <button @click="count++">Alpine Counter: <span x-text="count"></span></button>
        <p>Current time: #timeFormat(now(), "HH:mm:ss")#</p>
    </div>
    
    <button wire:click="refresh">Refresh Component</button>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:ignore Does

When you add `wire:ignore` to an element, CBWIRE automatically:

* **Skips DOM Updates**: Prevents the element and its children from being updated during re-renders
* **Preserves Third-Party State**: Maintains JavaScript library state and event listeners
* **Protects Static Content**: Keeps timestamps, IDs, and other content that should remain unchanged
* **Enables Library Integration**: Allows Alpine.js and other frameworks to manage their own DOM sections

## Available Modifiers

The `wire:ignore` directive supports one modifier:

* **Self**: `.self` - Ignores updates only to the element's attributes, not its children

Example: `wire:ignore.self` protects element attributes while allowing child content to update

{% hint style="info" %}
Use `wire:ignore` when integrating third-party JavaScript libraries or preserving content that should remain static across component updates.
{% endhint %}
