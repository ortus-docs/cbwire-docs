# wire:offline

The `wire:offline` directive enables you to create responsive user interfaces that gracefully handle network connectivity changes, providing visual feedback when users go offline or come back online.

## Basic Usage

This network status component demonstrates `wire:offline` for handling connectivity states, including content visibility, class toggling, and user messaging:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/NetworkStatus.bx
class extends="cbwire.models.Component" {
    data = {
        "lastSyncTime": "",
        "pendingChanges": false,
        "userMessage": ""
    };

    function onMount() {
        data.lastSyncTime = dateTimeFormat(now(), "medium");
    }

    function syncData() {
        // Simulate data sync when back online
        data.lastSyncTime = dateTimeFormat(now(), "medium");
        data.pendingChanges = false;
        data.userMessage = "Data synced successfully!";
    }

    function saveOffline() {
        // Handle offline saves
        data.pendingChanges = true;
        data.userMessage = "Changes saved locally. Will sync when online.";
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/NetworkStatus.cfc
component extends="cbwire.models.Component" {
    data = {
        "lastSyncTime" = "",
        "pendingChanges" = false,
        "userMessage" = ""
    };

    function onMount() {
        data.lastSyncTime = dateTimeFormat(now(), "medium");
    }

    function syncData() {
        // Simulate data sync when back online
        data.lastSyncTime = dateTimeFormat(now(), "medium");
        data.pendingChanges = false;
        data.userMessage = "Data synced successfully!";
    }

    function saveOffline() {
        // Handle offline saves
        data.pendingChanges = true;
        data.userMessage = "Changes saved locally. Will sync when online.";
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/networkStatus.bxm -->
<bx:output>
<div class="app-container">
    <header class="app-header">
        <h1>My Application</h1>
        
        <!-- Connection status indicator with class toggling -->
        <div wire:offline.class="offline" wire:offline.class.remove="online" 
             class="status-indicator online">
            <span class="status-dot"></span>
            <span class="status-text">Connected</span>
        </div>
    </header>

    <!-- Offline banner - only shows when offline -->
    <div wire:offline class="offline-banner">
        <h3>You're currently offline</h3>
        <p>Changes will be saved locally and synced when you reconnect.</p>
    </div>

    <!-- Main content area -->
    <main class="content">
        <div class="sync-info">
            <p>Last sync: #lastSyncTime#</p>
            <bx:if pendingChanges>
                <span class="pending-badge">Changes pending sync</span>
            </bx:if>
        </div>

        <div class="user-actions">
            <!-- Button behavior changes based on connection -->
            <button wire:offline.class="disabled" 
                    wire:click="syncData" 
                    class="sync-button">
                Sync Now
            </button>
            
            <button wire:click="saveOffline" class="save-button">
                Save Changes
            </button>
        </div>

        <!-- Status messages -->
        <bx:if userMessage.len()>
            <div class="message-box">
                #userMessage#
            </div>
        </bx:if>
    </main>

    <!-- Footer with connectivity-dependent content -->
    <footer wire:offline.class.remove="connected" class="app-footer connected">
        <p>Real-time updates enabled</p>
    </footer>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/networkStatus.cfm -->
<cfoutput>
<div class="app-container">
    <header class="app-header">
        <h1>My Application</h1>
        
        <!-- Connection status indicator with class toggling -->
        <div wire:offline.class="offline" wire:offline.class.remove="online" 
             class="status-indicator online">
            <span class="status-dot"></span>
            <span class="status-text">Connected</span>
        </div>
    </header>

    <!-- Offline banner - only shows when offline -->
    <div wire:offline class="offline-banner">
        <h3>You're currently offline</h3>
        <p>Changes will be saved locally and synced when you reconnect.</p>
    </div>

    <!-- Main content area -->
    <main class="content">
        <div class="sync-info">
            <p>Last sync: #lastSyncTime#</p>
            <cfif pendingChanges>
                <span class="pending-badge">Changes pending sync</span>
            </cfif>
        </div>

        <div class="user-actions">
            <!-- Button behavior changes based on connection -->
            <button wire:offline.class="disabled" 
                    wire:click="syncData" 
                    class="sync-button">
                Sync Now
            </button>
            
            <button wire:click="saveOffline" class="save-button">
                Save Changes
            </button>
        </div>

        <!-- Status messages -->
        <cfif len(userMessage)>
            <div class="message-box">
                #userMessage#
            </div>
        </cfif>
    </main>

    <!-- Footer with connectivity-dependent content -->
    <footer wire:offline.class.remove="connected" class="app-footer connected">
        <p>Real-time updates enabled</p>
    </footer>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:offline Does

When you add `wire:offline` to an element, CBWIRE automatically:

- **Detects connectivity changes**: Monitors browser online/offline status in real-time
- **Shows/hides elements**: Makes elements visible only when the user goes offline
- **Toggles CSS classes**: Adds or removes specified classes based on connection state
- **Provides immediate feedback**: Updates the UI instantly when connectivity changes

## Available Modifiers

You can customize offline behavior with these modifiers:

- **Class addition**: `.class="offline-style"` - Add CSS classes when offline
- **Class removal**: `.class.remove="online-style"` - Remove CSS classes when offline

Combine both modifiers: `wire:offline.class="offline".class.remove="online"`
