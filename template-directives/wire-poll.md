# wire:poll

Polling is a straightforward yet effective technique used in web applications to continuously request updates from the server at regular intervals. This method is essential for keeping page content fresh without the complexity of technologies like WebSockets.

In CBWIRE, implementing polling is as simple as adding **wire:poll** to an element in your component.

## Example: Real-time Subscriber Count

Here’s an example of a Subscriber Count component that dynamically updates the user's subscriber count:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
class extends="cbwire.models.Component" {

    data = {
        "count": 0
    };

    function refreshSubscribers() {
        // Simulate fetching subscriber count
        data.count = querySubscriberCount();
    }

    function renderIt() {
        return template( "wires.subscriberCount" );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
component extends="cbwire.models.Component" {

    data = {
        "count": 0
    };

    function refreshSubscribers() {
        // Simulate fetching subscriber count
        data.count = querySubscriberCount();
    }

    function renderIt() {
        return template( "wires.subscriberCount" );
    }
}
```
{% endtab %}
{% endtabs %}

In your subscriberCount view file (wires/subscriberCount.bxm or wires/subscriberCount.cfm):

```html
<div wire:poll="refreshSubscribers">
    Subscribers: #count#
</div>
```

Normally, the subscriber count would only update upon page refresh. However, with **wire:poll**, the **refreshSubscribers()** method is called every 2.5 seconds, ensuring the count is always up-to-date.

## Timing Control

Polling can be resource-intensive, particularly with many users. To manage this, CBWIRE allows you to control the polling interval:

```html
<!-- Poll every 15 seconds -->
<div wire:poll.15s>

<!-- Poll every 15000 milliseconds -->
<div wire:poll.15000ms>
```

## Background Throttling

CBWIRE intelligently throttles polling when the webpage is in the background. This reduces server load significantly. However, if continuous polling is necessary, you can use the **.keep-alive** modifier:

```html
<div wire:poll.keep-alive>
```

This ensures polling continues even when the tab is not active.

## Viewport Throttling

To optimize performance further, you can use the **.visible** modifier, which makes CBWIRE poll only when the element is visible on the user’s screen:

```html
<div wire:poll.visible>
```

This is useful for content lower on a page, as it starts polling only when scrolled into view and stops when it's no longer visible.
