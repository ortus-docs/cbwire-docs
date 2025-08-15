# wire:key

The `wire:key` directive helps Livewire's DOM diffing engine accurately track elements across re-renders. By providing unique identifiers for dynamic content, especially in loops, `wire:key` ensures that updates, removals, and reordering work correctly and efficiently.

## Basic Usage

This simple list demonstrates `wire:key` for proper DOM tracking in loops:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ItemList.bx
class extends="cbwire.models.Component" {
    data = {
        "items": [
            {"id": 1, "name": "Apple"},
            {"id": 2, "name": "Banana"},
            {"id": 3, "name": "Cherry"}
        ]
    };

    function removeItem(itemId) {
        data.items = data.items.filter(function(item) {
            return item.id != arguments.itemId;
        });
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ItemList.cfc
component extends="cbwire.models.Component" {
    data = {
        "items" = [
            {"id" = 1, "name" = "Apple"},
            {"id" = 2, "name" = "Banana"},
            {"id" = 3, "name" = "Cherry"}
        ]
    };

    function removeItem(itemId) {
        data.items = data.items.filter(function(item) {
            return item.id != arguments.itemId;
        });
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/itemList.bxm -->
<bx:output>
<div>
    <bx:loop array="#items#" index="item">
        <div wire:key="item-#item.id#">
            #item.name#
            <button wire:click="removeItem(#item.id#)">Remove</button>
        </div>
    </bx:loop>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/itemList.cfm -->
<cfoutput>
<div>
    <cfloop array="#items#" index="item">
        <div wire:key="item-#item.id#">
            #item.name#
            <button wire:click="removeItem(#item.id#)">Remove</button>
        </div>
    </cfloop>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:key Does

When you add `wire:key` to an element, CBWIRE automatically:

- **Tracks Element Identity**: Provides unique identifiers for DOM diffing across re-renders
- **Prevents Rendering Issues**: Ensures proper updates when elements are added, removed, or reordered
- **Optimizes Performance**: Helps Livewire efficiently update only changed elements
- **Maintains State**: Preserves element state and event listeners during DOM updates

## Available Modifiers

The `wire:key` directive does not support any modifiers.

{% hint style="warning" %}
Ensure your key names are unique throughout the entire page, not just within your component template. Duplicate keys can cause rendering issues.
{% endhint %}

{% hint style="info" %}
Use `wire:key` in loops and dynamic content where elements can be added, removed, or reordered. Also use it in conditional blocks when Livewire has trouble detecting DOM changes within if/else statements.
{% endhint %}