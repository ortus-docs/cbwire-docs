---
description: Toggle UI elements when the user is offline or online. Groovy!
---

# Offline State

You can use `wire:offline` to display elements when Livewire detects that the user is offline.

```html
<div wire:offline><!-- Oh no, you're offline --></div>
```

You can also append `.class` to your `wire:offline` [Directive](directives.md) and specify a CSS class to toggle when the user is offline.

```html
<div wire:offline.class="online" class="online"></div>
```

Use `.remove` to specify classes you want to be removed when offline.

```html
<div wire:offline.class.remove="im-online" class="im-online"></div>
```
