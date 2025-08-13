# wire:poll

The `wire:poll` directive provides an easy way to automatically refresh content at regular intervals, keeping your application's data current without requiring user interaction or complex real-time technologies.

## Basic Usage

This dashboard component demonstrates `wire:poll` for real-time data updates, including custom intervals, background throttling, and viewport-based polling:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/LiveDashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "subscriberCount": 0,
        "activeUsers": 0,
        "systemStatus": "operational",
        "notifications": []
    };

    function updateSubscribers() {
        // Fetch latest subscriber count
        data.subscriberCount = subscriberService.getCount();
    }

    function updateActiveUsers() {
        // Update active user count
        data.activeUsers = userService.getActiveCount();
    }

    function checkSystemStatus() {
        // Check system health
        data.systemStatus = systemService.getStatus();
    }

    function refreshNotifications() {
        // Get recent notifications
        data.notifications = notificationService.getRecent(5);
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/LiveDashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "subscriberCount" = 0,
        "activeUsers" = 0,
        "systemStatus" = "operational",
        "notifications" = []
    };

    function updateSubscribers() {
        // Fetch latest subscriber count
        data.subscriberCount = subscriberService.getCount();
    }

    function updateActiveUsers() {
        // Update active user count
        data.activeUsers = userService.getActiveCount();
    }

    function checkSystemStatus() {
        // Check system health
        data.systemStatus = systemService.getStatus();
    }

    function refreshNotifications() {
        // Get recent notifications
        data.notifications = notificationService.getRecent(5);
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/liveDashboard.bxm -->
<bx:output>
<div class="dashboard">
    <h1>Live Dashboard</h1>
    
    <!-- Basic polling every 2.5 seconds (default) -->
    <div wire:poll="updateSubscribers" class="metric-card">
        <h3>Subscribers</h3>
        <span class="count">#subscriberCount#</span>
    </div>
    
    <!-- Custom polling interval (every 5 seconds) -->
    <div wire:poll.5s="updateActiveUsers" class="metric-card">
        <h3>Active Users</h3>
        <span class="count">#activeUsers#</span>
    </div>
    
    <!-- Continuous polling even when tab is inactive -->
    <div wire:poll.10s.keep-alive="checkSystemStatus" class="status-card">
        <h3>System Status</h3>
        <span class="status #systemStatus#">#uCase(systemStatus)#</span>
    </div>
    
    <!-- Poll only when visible on screen -->
    <div wire:poll.15s.visible="refreshNotifications" class="notifications-card">
        <h3>Recent Notifications</h3>
        <bx:if notifications.len()>
            <ul>
                <bx:loop array="#notifications#" index="notification">
                    <li>#notification.message#</li>
                </bx:loop>
            </ul>
        <bx:else>
            <p>No recent notifications</p>
        </bx:if>
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/liveDashboard.cfm -->
<cfoutput>
<div class="dashboard">
    <h1>Live Dashboard</h1>
    
    <!-- Basic polling every 2.5 seconds (default) -->
    <div wire:poll="updateSubscribers" class="metric-card">
        <h3>Subscribers</h3>
        <span class="count">#subscriberCount#</span>
    </div>
    
    <!-- Custom polling interval (every 5 seconds) -->
    <div wire:poll.5s="updateActiveUsers" class="metric-card">
        <h3>Active Users</h3>
        <span class="count">#activeUsers#</span>
    </div>
    
    <!-- Continuous polling even when tab is inactive -->
    <div wire:poll.10s.keep-alive="checkSystemStatus" class="status-card">
        <h3>System Status</h3>
        <span class="status #systemStatus#">#uCase(systemStatus)#</span>
    </div>
    
    <!-- Poll only when visible on screen -->
    <div wire:poll.15s.visible="refreshNotifications" class="notifications-card">
        <h3>Recent Notifications</h3>
        <cfif arrayLen(notifications)>
            <ul>
                <cfloop array="#notifications#" index="notification">
                    <li>#notification.message#</li>
                </cfloop>
            </ul>
        <cfelse>
            <p>No recent notifications</p>
        </cfif>
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:poll Does

When you add `wire:poll` to an element, CBWIRE automatically:

- **Calls your method repeatedly**: Executes the specified component method at regular intervals
- **Updates content automatically**: Refreshes the display with new data from each polling request
- **Optimizes performance**: Intelligently throttles polling when the browser tab is inactive
- **Manages resources**: Uses a default 2.5-second interval that balances freshness with server load

## Available Modifiers

You can customize polling behavior with these modifiers:

- **Timing**: `.5s`, `.15s`, `.5000ms` - Set custom polling intervals
- **Background**: `.keep-alive` - Continue polling when browser tab is inactive  
- **Viewport**: `.visible` - Poll only when element is visible on screen

Combine modifiers as needed: `wire:poll.10s.keep-alive` or `wire:poll.15s.visible`
