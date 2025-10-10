# wire:offline

The `wire:offline` directive enables you to create responsive user interfaces that gracefully handle network connectivity changes, providing visual feedback when users go offline or come back online.

## Basic Usage

This simple component demonstrates `wire:offline` for showing content when offline, adding CSS classes, and removing CSS classes:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/OfflineDemo.bx
class extends="cbwire.models.Component" {
    data = {};
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/OfflineDemo.cfc
component extends="cbwire.models.Component" {
    data = {};
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/offlineDemo.bxm -->
<bx:output>
<div class="app">
    <!-- Connection status indicator -->
    <div wire:offline.class="offline" wire:offline.class.remove="online" 
         class="status-indicator online">
        <span class="status-text">Connected</span>
    </div>

    <!-- Offline message - only shows when offline -->
    <div wire:offline class="offline-banner">
        <h3>You're currently offline</h3>
        <p>Some features may be unavailable.</p>
    </div>

    <!-- Button that gets disabled when offline -->
    <button wire:offline.class="disabled" class="sync-button">
        Sync Data
    </button>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/offlineDemo.cfm -->
<cfoutput>
<div class="app">
    <!-- Connection status indicator -->
    <div wire:offline.class="offline" wire:offline.class.remove="online" 
         class="status-indicator online">
        <span class="status-text">Connected</span>
    </div>

    <!-- Offline message - only shows when offline -->
    <div wire:offline class="offline-banner">
        <h3>You're currently offline</h3>
        <p>Some features may be unavailable.</p>
    </div>

    <!-- Button that gets disabled when offline -->
    <button wire:offline.class="disabled" class="sync-button">
        Sync Data
    </button>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:offline Does

When you add `wire:offline` to an element, CBWIRE automatically:

* **Detects connectivity changes**: Monitors browser online/offline status in real-time
* **Shows/hides elements**: Makes elements visible only when the user goes offline
* **Toggles CSS classes**: Adds or removes specified classes based on connection state
* **Provides immediate feedback**: Updates the UI instantly when connectivity changes

## Available Modifiers

You can customize offline behavior with these modifiers:

* **Class addition**: `.class="offline-style"` - Add CSS classes when offline
* **Class removal**: `.class.remove="online-style"` - Remove CSS classes when offline

Combine both modifiers: `wire:offline.class="offline".class.remove="online"`
