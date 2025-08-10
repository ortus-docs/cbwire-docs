# wire:model

## Overview

You can use **wire:model** in your [templates](../the-essentials/templates.md) to bind to [data properties](../the-essentials/properties.md) with form inputs.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/ContactForm.bx
class extends="cbwire.models.Component" {
    data = {
        // default values
        "submitted": false,
        "name": "",
        "message": ""
    };
    function sendMessage() {
        data.submitted = true;
        bx:mail
            to="user@somedomain.com",
            from="user@anotherdomain.com",
            subject="Message from " & data.name
        {
            writeOutput( data.message )
        }
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/ContactForm.cfc
component extends="cbwire.models.Component" {
    data = {
        // default values
        "submitted": false,
        "name": "",
        "message": ""
    };
    function sendMessage() {
        data.submitted = true;
        cfmail(
            to="user@somedomain.com",
            from="user@anotherdomain.com",
            subject="Message from " & data.name
        ) {
            writeOutput( data.message )
        }
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/contactform.bxm --->
<bx:output>
    <form wire:submit="sendMessage">
        <label>
            <span>Name</span>
            <input type="text" wire:model="name"> 
        </label>
        <label>
            <span>Message</span>
            <textarea wire:model="message"></textarea> 
        </label>
        <button type="submit">Send Message</button>
        <cfif submitted>
            Sent
        </cfif>
    </form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/contactform.cfm --->
<cfoutput>
    <form wire:submit="sendMessage">
        <label>
            <span>Name</span>
            <input type="text" wire:model="name"> 
        </label>
        <label>
            <span>Message</span>
            <textarea wire:model="message"></textarea> 
        </label>
        <button type="submit">Send Message</button>
        <cfif submitted>
            Sent
        </cfif>
    </form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

When the user submits the form, the server receives a request with the updated data property values from the inputs. An email is sent, and the template is re-rendered with the updated values.

{% hint style="info" %}
When using **wire:model**, Livewire will only send a network request when an action is performed, like with [**wire:click**](wire-click.md) or [**wire:submit**](wire-submit.md)**.** However, you may want to update the server more frequently for things like real-time validation. In those instances, use **wire:model.live**.
{% endhint %}

## Live Updating

You can use **wire:model.live** to send property updates to the server as a user types.

```html
<input type="text" wire:model.live="name">
```

{% hint style="info" %}
By default, Livewire adds a 150-millisecond debounce to server updates. If the user is continually typing, Livewire will wait until the user stops typing for 150 milliseconds before sending a request.
{% endhint %}

You can customize the debounce using **wire:model.live.debounce.Xms**.

```html
<input type="text" wire:model.live.debounce.500ms="name">
```

## Modifiers

Livewire provides various modifiers to control when server requests are sent.

```html
<!--- Update onBlur(), great for validation --->
<input type="text" wire:model.blur="title">

<!--- Update onChange() --->
<select wire:model.change="option">
    ...
</select> 
```

Below are all the available modifiers:

| Modifier      | Description                                                                 |
| ------------- | --------------------------------------------------------------------------- |
| .live         | Send updates as user types                                                  |
| .blur         | Only send updates on blur event                                             |
| .change       | Only send updates on the change event                                       |
| .lazy         | Alias for .change                                                           |
| .debounce.Xms | Debounce sending updates for X milliseconds                                 |
| .throttle.Xms | Throttle network request updates by X milliseconds                          |
| .number       | Cast text input to an integer on the server                                 |
| .boolean      | Cast text input to a boolean on the server                                  |
| .fill         | Use the initial value the "value" HTML attribute provides during page load. |

## Input Fields

Below shows how to bind various input fields with your [data properties](../the-essentials/properties.md).

### Text inputs

```html
<input type="text" wire:model="name">
```

### Textarea inputs

```html
<textarea type="text" wire:model="content"></textarea>
```

{% hint style="warning" %}
If the **content** data property is initialized with a string, Livewire will fill the textarea with its value. Don't do this.

```html
<textarea type="text" wire:model="content">#content#</textarea>
```
{% endhint %}

### Single Checkbox

```html
<input type="checkbox" wire:model="receiveUpdates">
```

{% hint style="info" %}
If the **receiveUpdates** data property is **false**, the checkbox will be unchecked. If **true**, it will be checked.
{% endhint %}

### Multiple Checkboxes

Checkboxes with multiple inputs are stored as an array.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Options.bx
class extends="cbwire.models.Component" {
    data = {
        "selections": []
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Options.cfc
component extends="cbwire.models.Component" {
    data = {
        "selections": []
    };
}
```
{% endtab %}
{% endtabs %}

```html
<div>
    <input type="checkbox" value="email" wire:model="selections">
    <input type="checkbox" value="sms" wire:model="selections">
    <input type="checkbox" value="notification" wire:model="selections">
</div>
```

### Radio buttons

```html
<input type="radio" value="yes" wire:model="sendEmail">
<input type="radio" value="no" wire:model="sendEmail">
```

### Select dropdowns

```html
<select wire:model="state">
    <option value="AL">Alabama</option>
    <option value="AK">Alaska</option>
    <option value="AZ">Arizona</option>
    ...
</select>
```

{% hint style="info" %}
Livewire automatically adds a **selected** attribute to the option that matches the data property value.
{% endhint %}

You can dynamically populate these with values.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
class extends="cbwire.models.Component" {
    function getStates() {
        return queryExecute( "select id, label from states" );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
component extends="cbwire.models.Component" {
    function getStates() {
        return queryExecute( "select id, label from states" );
    }
}
```
{% endtab %}
{% endtabs %}

```html
<select wire:model="state">
    <option disabled value="">Select a state</option>
    <cfloop array="#getStates()#" index="state">
        <option value="#state.id#">#state.label#</option>
    </cfloop>
</select>
```

### Dependent select dropdowns

If you have a dropdown that is dependent on the value of another dropdown and needs to update, you can use [**wire:key**](wire-key.md) to ensure it updates.

```html
<!--- Select your state --->
<select wire:model.live="state">
    <option disabled value="">Select a state</option>
    <cfloop array="#getStates()#" index="state">
        <option value="#state.id#">#state.label#</option>
    </cfloop>
</select>
<!--- Select city based on state --->
<select wire:model.live="city" wire:key="#state#">
    <option disabled value="">Select a city</option>
    <cfloop array="#getCities( state )#" index="city">
        <option value="#city.id#">#city.label#</option>
    </cfloop>
</select>
```

### Multi-select dropdowns

```html
<select wire:model="states" multiple>
    <option value="AL">Alabama</option>
    <option value="AK">Alaska</option>
    <option value="AZ">Arizona</option>
    ...
</select>
```
