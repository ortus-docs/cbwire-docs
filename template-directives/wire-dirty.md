# wire:dirty

You can use **wire:dirty** with [wire:model](wire-model.md) to display elements when [data properties](../the-essentials/properties.md) have been updated on the client side but not the server side.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/MyForm.bx
class extends="cbwire.models.Component" {
    data = {
        "name": "",
        "email": ""
    };
    function submit() {
        // Do something useful here
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/MyForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "name": "",
        "email": ""
    };
    function submit() {
        // Do something useful here
    }
}
```
{% endtab %}
{% endtabs %}

```html
<!--- ./wires/myform.bxm|cfm --->
<form wire:submit="submit">
    <input type="text" wire:model="name">
    <input type="email" wire:model="email">
    <button type="submit">Submit</button>
    <div wire:dirty>
        Be sure to click submit. 👀
    </div> 
</form>
```

As the user begins typing in the name or email field, CBWIRE will detect the data properties that have changed on the client side and display a reminder to click submit. **After the user clicks submit, the data properties are synchronized, and the reminder is removed.**

## Removing Elements

You can invert the behavior of **wire:dirty** by using the **.remove** modifier:

```html
<div>
    <div wire:dirty.remove>Data is synced.</div>
</div>
```

## Targeting Properties

You can target specific data properties using **wire:dirty** and **wire:target**.

```html
<form wire:submit="submit">
    <input type="text" wire:model="name">
    <div wire:dirty wire:target="name">Unsaved name changes...</div>
    <button type="submit">Update</button>
</form>
```

## Toggling Classes

You can toggle classes on elements using **wire:dirty.class**.

```html
<input wire:model="name" wire:dirty.class="border-yellow-500">
```

When the user types into the name field, a yellow border is displayed, letting the user know the changes have not been saved.
