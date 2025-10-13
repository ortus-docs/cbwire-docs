# wire:replace

Livewire's DOM diffing is great for updating existing elements on your page, but occasionally you may need to force some elements to render from scratch to reset internal state.

In these cases, you can use the `wire:replace` directive to instruct Livewire to skip DOM diffing on the children of an element, and instead completely replace the content with the new elements from the server.

This is most useful in the context of working with third-party JavaScript libraries and custom web components, or when element re-use could cause problems when keeping state.

## Basic Usage

Below is an example of wrapping a web component with a shadow DOM with `wire:replace` so that Livewire completely replaces the element allowing the custom element to handle its own life-cycle:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/jsonViewer.bxm -->
<bx:output>
<form>
    <!-- ... -->

    <div wire:replace>
        <!-- This custom element would have its own internal state -->
        <json-viewer>#serializeJSON(someProperty)#</json-viewer>
    </div>

    <!-- ... -->
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/jsonViewer.cfm -->
<cfoutput>
<form>
    <!-- ... -->

    <div wire:replace>
        <!-- This custom element would have its own internal state -->
        <json-viewer>#serializeJSON(someProperty)#</json-viewer>
    </div>

    <!-- ... -->
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Replace Self

You can also instruct Livewire to replace the target element as well as all children with `wire:replace.self`.

```html
<div x-data="{open: false}" wire:replace.self>
  <!-- Ensure that the "open" state is reset to false on each render -->
</div>
```
