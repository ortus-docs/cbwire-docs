# wire:click

The `wire:click` directive transforms any HTML element into an interactive control that triggers server-side actions. Instead of traditional page refreshes or complex JavaScript event handling, `wire:click` lets you respond to user clicks by calling component methods directly, creating smooth, dynamic user experiences.

## Basic Usage

This counter component demonstrates `wire:click` with basic actions and parameter passing:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ClickDemo.bx
class extends="cbwire.models.Component" {
    data = {
        "count": 0,
        "message": ""
    };

    function increment() {
        data.count++;
    }

    function setMessage(text) {
        data.message = arguments.text;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ClickDemo.cfc
component extends="cbwire.models.Component" {
    data = {
        "count" = 0,
        "message" = ""
    };

    function increment() {
        data.count++;
    }

    function setMessage(text) {
        data.message = arguments.text;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/clickDemo.bxm -->
<bx:output>
<div>
    <h1>Count: #count#</h1>
    <button wire:click="increment">+</button>
    <button wire:click="setMessage('Hello!')">Say Hello</button>
    <a href="#home" wire:click.prevent="setMessage('Link clicked!')">Click Link</a>
    <p>#message#</p>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/clickDemo.cfm -->
<cfoutput>
<div>
    <h1>Count: #count#</h1>
    <button wire:click="increment">+</button>
    <button wire:click="setMessage('Hello!')">Say Hello</button>
    <a href="##home" wire:click.prevent="setMessage('Link clicked!')">Click Link</a>
    <p>#message#</p>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:click Does

When you add `wire:click` to an element, CBWIRE automatically:

* **Captures Click Events**: Listens for click events on the element without requiring JavaScript
* **Calls Component Methods**: Executes the specified action method in your component
* **Passes Parameters**: Supports passing arguments to your action methods using parentheses syntax
* **Updates UI**: Re-renders the component with any data changes from your action
* **Provides Loading States**: Integrates with `wire:loading` for visual feedback during actions

## Available Modifiers

The `wire:click` directive supports one modifier:

* **Prevent**: `.prevent` - Prevents default browser behavior (essential for links)

Example: `wire:click.prevent="sendEmail"`

{% hint style="info" %}
Use `wire:click` on any HTML element - buttons, links, divs, images, or any clickable element - making it incredibly versatile for building interactive interfaces.
{% endhint %}
