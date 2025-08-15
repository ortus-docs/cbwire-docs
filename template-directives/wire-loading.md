# wire:loading

The `wire:loading` directive shows and hides elements during server requests, providing instant visual feedback to users. When actions are processing, you can display loading indicators, disable buttons, or modify the interface to communicate that work is happening behind the scenes.

## Basic Usage

This form demonstrates `wire:loading` with various loading states and targeting options:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/DocumentManager.bx
class extends="cbwire.models.Component" {
    data = {
        "title": "",
        "content": ""
    };

    function save() {
        sleep(2000); // Simulate slow save
    }

    function delete() {
        sleep(1500); // Simulate deletion
    }

    function reset() {
        data.title = "";
        data.content = "";
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/DocumentManager.cfc
component extends="cbwire.models.Component" {
    data = {
        "title" = "",
        "content" = ""
    };

    function save() {
        sleep(2000); // Simulate slow save
    }

    function delete() {
        sleep(1500); // Simulate deletion
    }

    function reset() {
        data.title = "";
        data.content = "";
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/documentManager.bxm -->
<bx:output>
<form wire:submit="save" wire:loading.class="opacity-50">
    <input type="text" wire:model="title" placeholder="Title">
    <textarea wire:model="content" placeholder="Content"></textarea>
    
    <button type="submit" wire:loading.attr="disabled">Save</button>
    <button type="button" wire:click="delete" wire:loading.remove>Delete</button>
    <button type="button" wire:click="reset">Reset</button>
    
    <div wire:loading wire:target="save" wire:loading.delay>Saving document...</div>
    <div wire:loading wire:target="delete">Deleting...</div>
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/documentManager.cfm -->
<cfoutput>
<form wire:submit="save" wire:loading.class="opacity-50">
    <input type="text" wire:model="title" placeholder="Title">
    <textarea wire:model="content" placeholder="Content"></textarea>
    
    <button type="submit" wire:loading.attr="disabled">Save</button>
    <button type="button" wire:click="delete" wire:loading.remove>Delete</button>
    <button type="button" wire:click="reset">Reset</button>
    
    <div wire:loading wire:target="save" wire:loading.delay>Saving document...</div>
    <div wire:loading wire:target="delete">Deleting...</div>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:loading Does

When you add `wire:loading` to an element, CBWIRE automatically:

- **Shows Elements**: Displays loading indicators during server requests
- **Targets Actions**: Can target specific actions or all actions by default
- **Modifies Appearance**: Toggles classes, attributes, and display properties
- **Provides Feedback**: Gives users immediate visual confirmation that actions are processing

## Available Modifiers

The `wire:loading` directive supports these modifiers:

- **Remove**: `.remove` - Hides element during loading instead of showing it
- **Class**: `.class` - Toggles CSS classes during loading
- **Class Remove**: `.class.remove` - Removes specific classes during loading
- **Attr**: `.attr` - Toggles attributes like `disabled` during loading
- **Display Values**: `.block`, `.flex`, `.grid`, `.inline`, `.inline-block`, `.inline-flex`, `.table` - Sets specific display property
- **Delay**: `.delay` - Delays showing indicator (200ms default)
- **Delay Intervals**: `.delay.shortest` (50ms), `.delay.shorter` (100ms), `.delay.short` (150ms), `.delay.long` (300ms), `.delay.longer` (500ms), `.delay.longest` (1000ms)

## Targeting Actions

Use `wire:target` to specify which actions trigger loading states:

```html
<!-- Target specific action -->
<div wire:loading wire:target="save">Saving...</div>

<!-- Target multiple actions -->
<div wire:loading wire:target="save,delete">Processing...</div>

<!-- Target action with parameters -->
<div wire:loading wire:target="remove(123)">Removing item...</div>

<!-- Exclude specific actions -->
<div wire:loading wire:target.except="save">Working...</div>

<!-- Target property updates -->
<input wire:model.live="username">
<div wire:loading wire:target="username">Checking availability...</div>
```

{% hint style="danger" %}
Multiple action parameters are not supported: `wire:target="remove(1),add(1)"` will not work.
{% endhint %}

{% hint style="info" %}
By default, `wire:loading` triggers for any action. Use `wire:target` to control exactly when loading states appear.
{% endhint %}