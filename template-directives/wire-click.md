# wire:click

## Overview

You can listen for click events within your [templates](../the-essentials/templates.md) using **wire:click** and provide an [action](../the-essentials/actions.md) to run when clicked.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/MyForm.bx
class extends="cbwire.models.Component" {
    data = {
        "sent": false
    };
    function sendEmails(){
        sleep( 2000 ); // pretend this takes a while
        data.sent = true;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/MyForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "sent": false
    };
    function sendEmails(){
        sleep( 2000 ); // pretend this takes a while
        data.sent = true;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/myform.bxm --->
<div>
    <button type="button" wire:click="sendEmail">Send Important Emails</button>
    <cfif sent>
        <div>Check your spam folder. We spammed you.</div>
    </cfif>
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/myform.cfm --->
<div>
    <button type="button" wire:click="sendEmail">Send Important Emails</button>
    <cfif sent>
        <div>Check your spam folder. We spammed you.</div>
    </cfif>
</div>
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Add **wire:click** on any HTML elements, not just buttons and links.
{% endhint %}

## Using on links

When adding **wire:click** to links, you need to include the **.prevent** modifier to stop the default handling of a link in the browser. Otherwise, the browser will load the link and update the page's URL.

```html
<a href="#" wire:click.prevent="sendEmail">Click</a>
```
