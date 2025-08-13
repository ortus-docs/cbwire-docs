# wire:poll

The `wire:poll` directive provides an easy way to automatically refresh content at regular intervals, keeping your application's data current without requiring user interaction or complex real-time technologies.

## Basic Usage

This simple component demonstrates `wire:poll` with default intervals, custom timing, background polling, and viewport polling:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/PollDemo.bx
class extends="cbwire.models.Component" {
    data = {
        "counter": 0,
        "timestamp": ""
    };

    function updateCounter() {
        data.counter++;
        data.timestamp = dateTimeFormat(now(), "HH:mm:ss");
    }

    function updateStatus() {
        data.timestamp = dateTimeFormat(now(), "HH:mm:ss");
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/PollDemo.cfc
component extends="cbwire.models.Component" {
    data = {
        "counter" = 0,
        "timestamp" = ""
    };

    function updateCounter() {
        data.counter++;
        data.timestamp = dateTimeFormat(now(), "HH:mm:ss");
    }

    function updateStatus() {
        data.timestamp = dateTimeFormat(now(), "HH:mm:ss");
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/pollDemo.bxm -->
<bx:output>
<div>
    <!-- Basic polling every 2.5 seconds (default) -->
    <div wire:poll="updateCounter">
        <h3>Auto Counter: #counter#</h3>
        <p>Last updated: #timestamp#</p>
    </div>
    
    <!-- Custom polling interval -->
    <div wire:poll.5s="updateStatus">
        <h3>5-Second Updates</h3>
        <p>Current time: #timestamp#</p>
    </div>
    
    <!-- Keep polling when tab is inactive -->
    <div wire:poll.10s.keep-alive="updateStatus">
        <h3>Background Polling</h3>
        <p>Polls even when tab inactive</p>
    </div>
    
    <!-- Poll only when visible -->
    <div wire:poll.15s.visible="updateStatus">
        <h3>Visible Polling</h3>
        <p>Only polls when on screen</p>
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/pollDemo.cfm -->
<cfoutput>
<div>
    <!-- Basic polling every 2.5 seconds (default) -->
    <div wire:poll="updateCounter">
        <h3>Auto Counter: #counter#</h3>
        <p>Last updated: #timestamp#</p>
    </div>
    
    <!-- Custom polling interval -->
    <div wire:poll.5s="updateStatus">
        <h3>5-Second Updates</h3>
        <p>Current time: #timestamp#</p>
    </div>
    
    <!-- Keep polling when tab is inactive -->
    <div wire:poll.10s.keep-alive="updateStatus">
        <h3>Background Polling</h3>
        <p>Polls even when tab inactive</p>
    </div>
    
    <!-- Poll only when visible -->
    <div wire:poll.15s.visible="updateStatus">
        <h3>Visible Polling</h3>
        <p>Only polls when on screen</p>
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
