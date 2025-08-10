---
description: >-
  Poll for state changes based on a specified interval without page refreshes.
  Hot dog!
---

# Polling

You can add a `wire:poll` directive to your elements to poll for changes using a set interval. The default interval is set to poll every _2 seconds_.

```xml
<div wire:poll></div>
```

You can append a different interval time to your directive as well.

```xml
<div wire:poll.5s> <!-- poll every 5 seconds --> </div>
```

{% hint style="success" %}
Polling for changes over AJAX can be a resonable alternative to strategies such as Pusher or WebSockets.&#x20;
{% endhint %}

## Method Invocation

If you would like to invoke a method during each poll interval, you can do so by specifying a method name.

```xml
<div wire:poll="refreshTasks"></div>
```

```javascript
component extends="cbwire.models.Component" {
    
    function refreshTasks() {}
}
```

## Cancel Polling

If you want stop polling, you can simply no longer render the HTML element that has the `wire:poll` directive.

```html
<cfif args.shouldPoll>
    <div wire:poll="refreshTasks"></div>
<cfelse>
    <div><!-- No more polling --></div>
</cfif>
```
