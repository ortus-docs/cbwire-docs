# wire:submit

The `wire:submit` directive provides a seamless way to handle form submissions in your CBWIRE components. Instead of dealing with traditional form processing and page refreshes, `wire:submit` intercepts form submissions and calls your component methods directly, creating smooth, dynamic form experiences.

## Basic Usage

This contact form demonstrates `wire:submit` with form handling and loading states:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ContactForm.bx
class extends="cbwire.models.Component" {
    data = {
        "name": "",
        "email": ""
    };

    function submit() {
        // Process form (simulate processing)
        sleep(1000);
        
        // Reset form
        data.name = "";
        data.email = "";
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
        "email" = ""
    };

    function submit() {
        // Process form (simulate processing)
        sleep(1000);
        
        // Reset form
        data.name = "";
        data.email = "";
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
    <input type="text" wire:model="name" placeholder="Your Name">
    <input type="email" wire:model="email" placeholder="Email">
    
    <button type="submit">
        Send
        <span wire:loading>ing...</span>
    </button>
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/contactForm.cfm -->
<cfoutput>
<form wire:submit="submit">
    <input type="text" wire:model="name" placeholder="Your Name">
    <input type="email" wire:model="email" placeholder="Email">
    
    <button type="submit">
        Send
        <span wire:loading>ing...</span>
    </button>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:submit Does

When you add `wire:submit` to a form, CBWIRE automatically:

* **Prevents Default Submission**: Stops the browser from submitting the form traditionally
* **Calls Your Method**: Executes the specified component method when the form is submitted
* **Disables Form Elements**: Automatically disables submit buttons and marks form inputs as readonly during processing
* **Maintains State**: Keeps all your component data intact during the submission process

## Available Modifiers

You can customize form submission behavior with these modifiers:

* **Prevent**: `.prevent` - Explicitly prevent default form submission (default behavior)

Example: `wire:submit.prevent="saveUser"`
