# wire:dirty

The `wire:dirty` directive shows or hides elements based on whether form data has been modified but not yet synced to the server. This provides instant visual feedback to users about unsaved changes, helping prevent data loss and improving the form experience.

## Basic Usage

This contact form demonstrates `wire:dirty` with unsaved change indicators and targeted property tracking:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ContactForm.bx
class extends="cbwire.models.Component" {
    data = {
        "name": "",
        "email": "",
        "message": ""
    };

    function submit() {
        // Save form data
        sleep(1000);
        // Form is now synced
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ContactForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "",
        "email" = "",
        "message" = ""
    };

    function submit() {
        // Save form data
        sleep(1000);
        // Form is now synced
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/contactForm.bxm -->
<bx:output>
<form wire:submit="submit">
    <input type="text" wire:model="name" wire:dirty.class="border-yellow-500" placeholder="Name">
    <div wire:dirty wire:target="name">Name has unsaved changes...</div>
    
    <input type="email" wire:model="email" placeholder="Email">
    
    <textarea wire:model="message" placeholder="Message"></textarea>
    
    <button type="submit">Submit</button>
    
    <div wire:dirty>You have unsaved changes. Click submit to save.</div>
    <div wire:dirty.remove>All changes saved!</div>
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/contactForm.cfm -->
<cfoutput>
<form wire:submit="submit">
    <input type="text" wire:model="name" wire:dirty.class="border-yellow-500" placeholder="Name">
    <div wire:dirty wire:target="name">Name has unsaved changes...</div>
    
    <input type="email" wire:model="email" placeholder="Email">
    
    <textarea wire:model="message" placeholder="Message"></textarea>
    
    <button type="submit">Submit</button>
    
    <div wire:dirty>You have unsaved changes. Click submit to save.</div>
    <div wire:dirty.remove>All changes saved!</div>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:dirty Does

When you add `wire:dirty` to an element, CBWIRE automatically:

- **Tracks Form Changes**: Monitors when form inputs differ from server-side values
- **Shows Elements**: Displays elements when data properties have unsaved changes
- **Hides When Synced**: Removes elements after data is synchronized with the server
- **Integrates with wire:model**: Works seamlessly with two-way data binding

## Available Modifiers

The `wire:dirty` directive supports these modifiers:

- **Remove**: `.remove` - Inverts behavior, showing element when data is synced
- **Class**: `.class` - Toggles CSS classes instead of showing/hiding elements

Combine with `wire:target` to track specific properties: `wire:dirty wire:target="name"`

{% hint style="info" %}
Use `wire:dirty` with forms to provide clear feedback about unsaved changes and prevent accidental data loss.
{% endhint %}