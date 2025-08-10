---
description: >-
  You can emit events from your cbwire components, views, and also direct from
  JavaScript. Listeners can be defined to execute specific actions.
---

# Events

You can fire events from within views, components, or by using the global `cbwire` JavaScript object.

### From View

```markup
<button wire:click="$emit( 'counterIncremented' )">
```

### From Component

```javascript
this.emit( "counterIncremented" );
```

### From Javascript Global

```xml
<script>
    cbwire.emit( 'counterIncremented' );
</script>
```

## Event Listeners

You can register event listeners on a component by defining `variables.listeners`.

```javascript
component extends="cbwire.models.Component"{

	variables.listeners = {
		"counterIncremented": "tweetAboutIt"
	};

	function tweetAboutIt(){
		// Blow up Twitter
	}

	...
}
```

{% hint style="success" %}
cbwire will invoke the `tweetAboutIt` method on the component if any other component on the same page emits a `counterIncremented` event.&#x20;
{% endhint %}

{% hint style="info" %}
When defining your listeners, it is good to put your listener's names in quotation marks, as we have in our component above. \
\
`"counterIncremented": "tweetAboutIt"`

JavaScript keys are case-sensitive. You can preserve the key casing in CFML by surrounding your listener names in quotations. Without the quotations, CFML will convert the key to all uppercase, such as `TWEETABOUTIT`. :upside\_down:&#x20;
{% endhint %}

## Event Listeners In JavaScript

```javascript
// File: ./views/wires/myView.cfm
<script>
    cbwire.on( 'counterIncremented', counter => {
        alert( 'The counter was incremented with a value of: ' + counter );
    })
</script>
```
