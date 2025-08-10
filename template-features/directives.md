---
description: >-
  CBWIRE's powerful directives allow you to listen for client side events,
  invoke actions, bind to data properties, and more.
---

# Directives

## Core Directives

The following Directives can be added to your [Templates](../essentials/templates.md) and instruct CBWIRE how and when to update the [Wire](../essentials/creating-components.md).

### wire:key

```html
<div wire:key="foo"></div>
```

Define a key name `foo` for the element. This provides a reference point for the Livewire DOM diffing system. Useful for adding/removing elements and keeping track of lists.

### wire:click

```html
<button wire:click="foo">...</button>

<a href="" wire:click.prevent="foo">Click here</a>
```

Listens for a `click` event and invokes the `foo` [Action](../essentials/actions.md).

{% hint style="info" %}
You will want to add the .prevent [modifier](directives.md#event-modifiers) to ensure that the browser doesn't follow the link.
{% endhint %}

### wire:click.prefetch

```html
<button wire:click.prefetch="foo">...</button>
```

Listens for a `mouseEnter` event and then prefetches the result of the `foo` [Action](../essentials/actions.md). If the element is then clicked, it will swap in the prefetched result without any extra request. If it's not clicked, the cached results will be thrown away.

### wire:submit

```html
<form wire:submit.prevent="someAction">
    <button type="submit">Submit Form</button>
</form>
```

Listeners for a `submit` event on a form.

{% hint style="info" %}
You will want to add the .prevent [modifier](directives.md#event-modifiers) to ensure that the browser doesn't submit the form.
{% endhint %}

### wire:keydown

```html
<input wire:keydown="foo" type="text">
```

Listens for a `keyDown` event and invokes the `foo` [Action](../essentials/actions.md).

### wire:keydown.enter

```html
<input wire:keydown.enter="foo" type="text">
```

Listens for a `keyDown` event when the user hits enter and invokes the `foo` [Action](../essentials/actions.md).

### wire:foo

```html
<select wire:foo="bar"></select>
```

Listens for a `foo` event and invokes the `bar` [Action](../essentials/actions.md).&#x20;

{% hint style="success" %}
You can listen to any JS event, not just those defined by Livewire.
{% endhint %}

### wire:model

```html
<input wire:model="foo" type="text">
```

Provided you have `data[ "foo" ]` defined in your [Data Properties](../essentials/properties.md), this creates a one-to-one model binding. Any time the element is updated, the value is synchronized.

### wire:model.debounce

```html
<input wire:model.debounce.1s="foo" type="text">
<input wire:model.debounce.500ms="foo" type="text">
```

The same as `wire:model="foo"` except that all input events to the element will be debounced for the specified duration. Defaults to 150 milliseconds. Useful for reducing XHR background requests during user input. You can set the milliseconds value to any numeric value.

### wire:model.defer

```html
<input wire:model.defer="foo" type="text">
```

Creates a one-to-one model binding with `data[ "foo" ]` in your [Data Properties](../essentials/properties.md) but defers any XHR updates until an [Action](../essentials/actions.md) is performed. Useful for reducing XHR requests.

### wire:model.lazy

```
<input wire:model.lazy="foo" type="text">
```

Creates a one-to-one model binding with `data[ "foo" ]` in your [Data Properties](../essentials/properties.md) but will not perform an XHR updates until an `onBlur` event is emitted. Useful for reducing XHR requests.

{% hint style="success" %}
If your input field does not require immediate updating ( such as for instant form validation), we highly recommended you used the lazy modifier to reduce the number of XHR requests .
{% endhint %}

### wire:poll

```html
<div wire:poll></div>
<div wire:poll.5s></div>
<div wire:poll.5000ms></div>
<div wire:poll.5s="fooMethod"></div>
```

Performs an XHR request to re-render the elements based on a set interval. An interval can be specified in both seconds or milliseconds. You can also specify an [Action](../essentials/actions.md) that you want to invoke. [_See Polling_](polling.md)

### wire:init

```html
<div wire:init="foo"></div>
```

Invokes the `foo` [Action](../essentials/actions.md) on your [Wire](../essentials/creating-components.md) immediately after it's rendered on the page.

### wire:loading

```html
<span wire:loading><img src="spinner.gif"></span>
```

Hides the HTML element by default and makes it visible when XHR requests are performed.

### wire:loading.class

```html
<div wire:loading.class="highlight"></div>
```

Adds the `foo` class to the HTML element while XHR requests are in transit.

### wire:loading.class.remove

```html
<div wire:loading.class.remove="highlight" class="highlight"></div>
```

Removes the `foo` class from the HTML element while XHR requests are in transit.

### wire:loading.attr

```html
<button wire:loading.attr="disabled">...</button>
```

Adds the `disabled="true"` attribute while XHR requests are in transit.

### wire:dirty

Hides the HTML element by default and makes it visible when the element's state has changed since the latest XHR request.

```html
<div wire:dirty="foo">...</div>
```

### wire:dirty.class

```html
<div wire:dirty.class="highlight">...</div>
```

Adds the `foo` class to the HTML element when the element's state has changed since the latest XHR request.

### wire:dirty.class.remove

```html
<div wire:dirty.class.remove="highlight" class="highlight">...</div>
```

Removes the `foo` class from the HTML element when the element's state has changed since the latest XHR request.

### wire:dirty.attr

```html
<button wire:dirty.attr="disabled">...</div>
```

Adds the `disabled="true"` attribute when the element's state has changed since the latest XHR request. Used in addition to `wire:target`.

### wire:target

```html
<button wire:dirty.attr="disabled" wire:target="foo">...</div>
```

Provides scoping for `wire:loading` and `wire:dirty` references, scoped to a specific [Action](../essentials/actions.md).

### wire:ignore

```html
<div wire:ignore></div>
```

Instructs Livewire to not update the element or any child elements when updating the DOM. Useful when using third-party JavaScript libraries.

### wire:ignore.self

```html
<div wire:ignore.self></div>
```

Instructs Livewire to not update the element but DOES allow updates to any child elements when updating the DOM.

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

To listen for specific keys on **keydown** events, you can provide the name of the key as a modifier. You can use any valid key names exposed via [KeyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) as modifiers by converting them to kebab-case.

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

In the above example, the foo [Action](../essentials/actions.md) will only be called if `event.key` is equal to 'PageDown'.
