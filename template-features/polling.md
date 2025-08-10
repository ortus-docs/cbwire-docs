---
description: Update your components automatically based on a set time interval.
---

# Polling

You can add a **wire:poll** directive to your elements to poll for changes using a set interval. The default interval is set to poll every 2 seconds.

```xml
<div wire:poll></div>
```

You can append a different interval time to your directive as well.

```xml
<div wire:poll.5s> <!-- poll every 5 seconds --> </div>
```

{% hint style="success" %}
Polling for changes over AJAX can be a reasonable alternative to strategies such as Pusher or WebSockets.&#x20;
{% endhint %}

## Method Invocation

If you would like to invoke a method during each poll interval, you can do so by specifying a method name.

```xml
<cfoutput>
    <div>
        <div wire:poll="refreshTasks"></div>
    </div>
</cfoutput>

<cfscript>
    function refreshTask() {}
</cfscript>
```

## Stop Polling

If you want to stop polling, you can no longer render the HTML element by omitting the wire:poll directive.

```html
<cfif shouldPoll>
    <div wire:poll="refreshTasks"></div>
<cfelse>
    <div><!-- No more polling --></div>
</cfif>
```
