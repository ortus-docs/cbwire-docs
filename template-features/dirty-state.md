---
description: >-
  Display HTML elements as data properties change but haven't been updated
  server-side yet.
---

# Dirty State

In some cases, providing feedback that content has changed and needs to be synchronized with the back end may be helpful.

For input that uses _wire:model_, or _wire:model.lazy_, you can use _wire:dirty_ along with _wire:target_ to display HTML elements if a property has been changed client-side but not yet synced with the server.

```html
<div>
    <input wire:model.lazy="foo">
    <div wire:dirty wire:target="foo">Foo not synced yet</div>
</div>
```

## Class Modifier

Adding the _.class_ modifier allows you to add a class to the element when dirty.

```html
<div>
    <input wire:dirty.class="border-red-500" wire:model.lazy="foo">
</div>
```

You can perform the inverse and remove classes by adding the _.remove_ modifier.

```html
<div>
    <input wire:dirty.class.remove="bg-green-200" class="bg-green-200" wire:model.lazy="foo">
</div>
```
