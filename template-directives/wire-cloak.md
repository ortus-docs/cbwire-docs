# wire:cloak

`wire:cloak` is a directive that hides elements on page load until Livewire is fully initialized. This is useful for preventing the "flash of unstyled content" that can occur when the page loads before Livewire has a chance to initialize.

## Basic Usage

To use `wire:cloak`, add the directive to any element you want to hide during page load:

```html
<div wire:cloak>
    This content will be hidden until Livewire is fully loaded
</div>
```

## Dynamic Content

`wire:cloak` is particularly useful in scenarios where you want to prevent users from seeing uninitialized dynamic content such as elements shown or hidden using `wire:show`.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/togglePanel.bxm -->
<bx:output>
<div>
    <div wire:show="expanded" wire:cloak>
        <!-- Chevron down icon... -->
    </div>

    <div wire:show="!expanded" wire:cloak>
        <!-- Chevron right icon... -->
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/togglePanel.cfm -->
<cfoutput>
<div>
    <div wire:show="expanded" wire:cloak>
        <!-- Chevron down icon... -->
    </div>

    <div wire:show="!expanded" wire:cloak>
        <!-- Chevron right icon... -->
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

In the above example, without `wire:cloak`, both icons would be shown before Livewire initializes. However, with `wire:cloak`, both elements will be hidden until initialization.
