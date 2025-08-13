# wire:ignore

You can tell CBWIRE to ignore updating parts of your [templates](../the-essentials/templates.md) using **wire:ignore**. This is especially useful when using Alpine.js and other third-party libraries.

```html
<div>
    <div wire:ignore>
        <!--- This timestamp will never update --->
        Component first loaded #now()#.
    </div>
    <div>
        <!--- This timestamp will update on every refresh --->
        Component last updated #now()#.
    </div>
    <button wire:click="$refresh">Refresh</button>
</div>
```

## Ignoring Self

Using **wire:ignore.self**, you can tell CBWIRE to ignore changes only to the attributes of the root element and not its contents.&#x20;

```html
<div wire:ignore.self>
    ...
</div>
```
