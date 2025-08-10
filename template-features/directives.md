---
description: HTML directives to make your app come alive.
---

# Directives

CBWIRE's powerful directives allow you to listen for client side events, invoke actions, bind to data properties, and more.

The following directives can be added to your components and instruct CBWIRE how and when to update and render the component.

### wire:key

```html
<div wire:key="foo"></div>
```

Define a key name _foo_ for the element. This provides a reference point for the CBWIRE DOM diffing system. Useful for adding/removing elements and keeping track of lists.

{% hint style="success" %}
If you are working with nested components and the DOM is not updating as expected, adding wire:key with a unique identifier for each child component can often resolve the issue.
{% endhint %}

### wire:click

```html
<button wire:click="foo">...</button>

<a href="" wire:click.prevent="foo">Click here</a>
```

Listens for a _click_ event and invokes the _foo_ action.

{% hint style="info" %}
You will want to add the .prevent [modifier](directives.md#event-modifiers) to ensure that the browser doesn't follow the link.
{% endhint %}

### wire:click.prefetch

```html
<button wire:click.prefetch="foo">...</button>
```

Listens for a _mouseEnter_ event and then prefetches the result of the _foo_ action. If the element is then clicked, it will swap in the prefetched result without any extra request. If it's not clicked, the cached results will be thrown away.

### wire:submit

```html
<form wire:submit.prevent="someAction">
    <button type="submit">Submit Form</button>
</form>
```

Listeners for a _submit_ event on a form.

{% hint style="info" %}
You will want to add the _.prevent_ [modifier](directives.md#event-modifiers) to ensure that the browser doesn't submit the form.
{% endhint %}

### wire:keydown

```html
<input wire:keydown="foo" type="text">
```

Listens for a _keyDown_ event and invokes the _foo_ action.

### wire:keydown.enter

```html
<input wire:keydown.enter="foo" type="text">
```

Listens for a _keyDown_ event when the user hits enter and invokes the _foo_ action.

### wire:foo

```html
<select wire:foo="bar"></select>
```

Listens for a _foo_ event and invokes the _bar_ action.

{% hint style="success" %}
You can listen to any JS event, not just those defined by CBWIRE.
{% endhint %}

### wire:model

```html
<input wire:model="foo" type="text">
```

Provided you have _data.foo_ defined in your data properties, this creates a one-to-one model binding. Any time the element is updated, the value is synchronized.

### wire:model.debounce

```html
<input wire:model.debounce.1s="foo" type="text">
<input wire:model.debounce.500ms="foo" type="text">
```

The same as _wire:model="foo"_ except that all input events to the element will be debounced for the specified duration. Defaults to 150 milliseconds. Useful for reducing XHR background requests during user input. You can set the milliseconds value to any numeric value.

### wire:model.defer

```html
<input wire:model.defer="foo" type="text">
```

Creates a one-to-one model binding with _data.foo_ in your data properties but defers any XHR updates until an action is performed. Useful for reducing XHR requests.

### wire:model.lazy

```
<input wire:model.lazy="foo" type="text">
```

Creates a one-to-one model binding with _data.foo_ in your data properties but will not perform XHR updates until an _onBlur_ event is emitted. Urecommendseful for reducing XHR requests.

{% hint style="success" %}
If your input field does not require immediate updating ( such as for instant form validation), we highly recommend you use the _.lazy_ modifier to reduce the number of XHR requests.
{% endhint %}

### wire:poll

```html
<div wire:poll></div>
<div wire:poll.5s></div>
<div wire:poll.5000ms></div>
<div wire:poll.5s="fooMethod"></div>
```

Performs an XHR request to re-render the elements based on a set interval. An interval can be specified in both seconds or milliseconds. You can also specify an action that you want to invoke. [_See Polling_](polling.md)

### wire:init

```html
<div wire:init="foo"></div>
```

Invokes the _foo_ action on your component immediately after it's rendered on the page.

### wire:loading

```html
<span wire:loading><img src="spinner.gif"></span>
```

Hides the HTML element by default and makes it visible when XHR requests are performed.

### wire:loading.class

```html
<div wire:loading.class="highlight"></div>
```

Adds the _foo_ class to the HTML element while XHR requests are in transit.

### wire:loading.class.remove

```html
<div wire:loading.class.remove="highlight" class="highlight"></div>
```

Removes the _foo_ class from the HTML element while XHR requests are in transit.

### wire:loading.attr

```html
<button wire:loading.attr="disabled">...</button>
```

Adds the _disabled="true"_ attribute while XHR requests are in transit.

### wire:dirty

Hides the HTML element by default and makes it visible when the element's state has changed since the latest XHR request.

```html
<div wire:dirty="foo">...</div>
```

### wire:dirty.class

```html
<div wire:dirty.class="highlight">...</div>
```

Adds the _foo_ class to the HTML element when the element's state has changed since the latest XHR request.

### wire:dirty.class.remove

```html
<div wire:dirty.class.remove="highlight" class="highlight">...</div>
```

Removes the _foo_ class from the HTML element when the element's state has changed since the latest XHR request.

### wire:dirty.attr

```html
<button wire:dirty.attr="disabled">...</div>
```

Adds the _disabled="true"_ attribute when the element's state has changed since the latest XHR request. Used in addition to _wire:target_.

### wire:target

```html
<button wire:dirty.attr="disabled" wire:target="foo">...</div>
```

Provides scoping for _wire:loading_ and _wire:dirty_ references, scoped to a specific action.

### wire:ignore

```html
<div wire:ignore></div>
```

Instructs CBWIRE not to update the element or child elements when updating the DOM. Useful when using third-party JavaScript libraries.

### wire:ignore.self

```html
<div wire:ignore.self></div>
```

Instructs CBWIRE to not update the element but DOES allow updates to any child elements when updating the DOM.

### wire:offline

_See_ [_Offline State_](offline-state.md)

## Event Modifiers

CBWIRE Directives sometimes offer "modifiers" to add extra functionality to an event. Here are the available modifiers that can be used with any event.

| Modifier       |                                                                                                                                                                                                                                  |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| stop           | Equivalent of `event.stopPropagation()`                                                                                                                                                                                          |
| prevent        | Equivalent of `event.preventDefault()`                                                                                                                                                                                           |
| self           | Only triggers an action if the event was triggered on itself. This prevents outer elements from catching events that were triggered from a child element. (Like often in the case of registering a listener on a modal backdrop) |
| debounce.300ms | Adds an Xms debounce to the handling of the action.                                                                                                                                                                              |

```html
<a href="" wire:click.prevent="someAction">Click here</a>
```

### Keydown Modifiers

To listen for specific keys on _keydown_ events, you can provide the name of the key as a modifier. You can use any valid key names exposed via [KeyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) as modifiers by converting them to kebab-case.

Here is a quick list of some common ones you may need:

| Native Browser Event | Livewire Modifier |
| -------------------- | ----------------- |
| Backspace            | backspace         |
| Escape               | escape            |
| Shift                | shift             |
| Tab                  | tab               |
| ArrowRight           | arrow-right       |

```html
<input wire:keydown.page-down="foo">
```

In the above example, the _foo_ action will only be called if _event.key_ is equal to '_PageDown_'.
