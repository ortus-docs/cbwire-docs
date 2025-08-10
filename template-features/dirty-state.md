---
description: Track changes to Data Properties and display changes in your UI instantly.
---

# Dirty State

There are cases where it may be helpful to provide feedback that content has changed and is not yet in sync with the back-end. For input that uses `wire:model`, or `wire:model.lazy,` you can display that a field is 'dirty' until CBWIRE has fully updated.

### Toggle Dirty Elements

Adding the `.class` modifier allows you to add a class to the element when dirty.

```html
<div>
    <input wire:dirty.class="border-red-500" wire:model.lazy="foo">
</div>
```

You can perform the inverse and remove classes by adding the `.remove` modifier.

```html
<div>
    <input wire:dirty.class.remove="bg-green-200" class="bg-green-200" wire:model.lazy="foo">
</div>
```

### Toggle Elements

The default behavior of the `wire:dirty` directive without modifiers is that the element is hidden until dirty. This can create a paradox if used on the input itself, but like loading states, the `dirty` directive can be used to toggle the appearance of other elements using `wire:target.`

In this example, the `span` is hidden by default and only visible when the input element is dirty.

```html
<div>
    <span wire:dirty wire:target="foo">Updating...</span>
    <input wire:model.lazy="foo">
</div>
```

### Toggle Other Elements

Use the class and attribute modifiers in the same way for referenced elements.

```html
<div>
    <label wire:dirty.class="text-red-500" wire:target="foo">Full Name</label>
    <input wire:model.lazy="foo">
</div>
```
