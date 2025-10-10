# wire:model

The `wire:model` directive creates two-way data binding between form inputs and component data properties. As users type or interact with form elements, the values are automatically synchronized with your server-side data, enabling reactive forms without manual event handling.

## Basic Usage

This user profile form demonstrates `wire:model` with various input types and live updating:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserProfile.bx
class extends="cbwire.models.Component" {
    data = {
        "name": "",
        "email": "",
        "bio": "",
        "notifications": true,
        "preferences": [],
        "country": ""
    };

    function save() {
        // Save user profile
        sleep(1000);
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "",
        "email" = "",
        "bio" = "",
        "notifications" = true,
        "preferences" = [],
        "country" = ""
    };

    function save() {
        // Save user profile
        sleep(1000);
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userProfile.bxm -->
<bx:output>
<form wire:submit="save">
    <input type="text" wire:model.live.debounce.300ms="name" placeholder="Name">
    <input type="email" wire:model.blur="email" placeholder="Email">
    <textarea wire:model="bio" placeholder="Bio"></textarea>
    
    <input type="checkbox" wire:model="notifications"> Email notifications
    
    <input type="checkbox" value="updates" wire:model="preferences"> Product updates
    <input type="checkbox" value="marketing" wire:model="preferences"> Marketing
    
    <select wire:model="country">
        <option value="">Select country</option>
        <option value="US">United States</option>
        <option value="CA">Canada</option>
    </select>
    
    <button type="submit">Save Profile</button>
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userProfile.cfm -->
<cfoutput>
<form wire:submit="save">
    <input type="text" wire:model.live.debounce.300ms="name" placeholder="Name">
    <input type="email" wire:model.blur="email" placeholder="Email">
    <textarea wire:model="bio" placeholder="Bio"></textarea>
    
    <input type="checkbox" wire:model="notifications"> Email notifications
    
    <input type="checkbox" value="updates" wire:model="preferences"> Product updates
    <input type="checkbox" value="marketing" wire:model="preferences"> Marketing
    
    <select wire:model="country">
        <option value="">Select country</option>
        <option value="US">United States</option>
        <option value="CA">Canada</option>
    </select>
    
    <button type="submit">Save Profile</button>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:model Does

When you add `wire:model` to an input, CBWIRE automatically:

* **Binds Data**: Creates two-way binding between input values and component properties
* **Syncs Changes**: Updates server data when users interact with form elements
* **Preserves State**: Maintains input values across component re-renders
* **Handles Types**: Automatically manages different input types (text, checkbox, select, etc.)

## Available Modifiers

The `wire:model` directive supports these modifiers:

* **Live**: `.live` - Sends updates as user types (with 150ms debounce)
* **Blur**: `.blur` - Only sends updates when input loses focus
* **Change**: `.change` - Only sends updates on change event
* **Lazy**: `.lazy` - Prevents server updates until an action is called
* **Debounce**: `.debounce.Xms` - Customizes debounce timing (e.g., `.debounce.500ms`)
* **Throttle**: `.throttle.Xms` - Throttles network requests by X milliseconds
* **Number**: `.number` - Casts text input to integer on server
* **Boolean**: `.boolean` - Casts text input to boolean on server
* **Fill**: `.fill` - Uses initial HTML `value` attribute during page load

Combine modifiers as needed: `wire:model.live.debounce.500ms="username"`

## Input Types

### Text Inputs

```html
<input type="text" wire:model="name">
<input type="email" wire:model="email">
<input type="password" wire:model="password">
```

### Textarea

```html
<textarea wire:model="content"></textarea>
```

{% hint style="warning" %}
Don't initialize textarea content with the property value in the template:

```html
<!-- ❌ Don't do this -->
<textarea wire:model="content">#content#</textarea>

<!-- ✅ Do this instead -->
<textarea wire:model="content"></textarea>
```
{% endhint %}

### Single Checkbox

```html
<input type="checkbox" wire:model="receiveUpdates">
```

The checkbox will be checked if the data property is `true`, unchecked if `false`.

### Multiple Checkboxes

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// Component data
data = {
    "selections": []
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Component data
data = {
    "selections" = []
};
```
{% endtab %}
{% endtabs %}

```html
<input type="checkbox" value="email" wire:model="selections">
<input type="checkbox" value="sms" wire:model="selections">
<input type="checkbox" value="notification" wire:model="selections">
```

### Radio Buttons

```html
<input type="radio" value="yes" wire:model="sendEmail">
<input type="radio" value="no" wire:model="sendEmail">
```

### Select Dropdowns

```html
<select wire:model="state">
    <option value="AL">Alabama</option>
    <option value="AK">Alaska</option>
    <option value="AZ">Arizona</option>
</select>
```

Livewire automatically adds the `selected` attribute to the matching option.

### Dynamic Select Options

{% tabs %}
{% tab title="BoxLang" %}
```javascript
function getStates() {
    return queryExecute("select id, label from states");
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
function getStates() {
    return queryExecute("select id, label from states");
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<select wire:model="state">
    <option disabled value="">Select a state</option>
    <bx:loop array="#getStates()#" index="state">
        <option value="#state.id#">#state.label#</option>
    </bx:loop>
</select>
```
{% endtab %}

{% tab title="CFML" %}
```html
<select wire:model="state">
    <option disabled value="">Select a state</option>
    <cfloop array="#getStates()#" index="state">
        <option value="#state.id#">#state.label#</option>
    </cfloop>
</select>
```
{% endtab %}
{% endtabs %}

### Dependent Dropdowns

Use `wire:key` to ensure dependent dropdowns update properly:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- State selector -->
<select wire:model.live="state">
    <option disabled value="">Select a state</option>
    <bx:loop array="#getStates()#" index="state">
        <option value="#state.id#">#state.label#</option>
    </bx:loop>
</select>

<!-- City selector (depends on state) -->
<select wire:model.live="city" wire:key="#state#">
    <option disabled value="">Select a city</option>
    <bx:loop array="#getCities(state)#" index="city">
        <option value="#city.id#">#city.label#</option>
    </bx:loop>
</select>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- State selector -->
<select wire:model.live="state">
    <option disabled value="">Select a state</option>
    <cfloop array="#getStates()#" index="state">
        <option value="#state.id#">#state.label#</option>
    </cfloop>
</select>

<!-- City selector (depends on state) -->
<select wire:model.live="city" wire:key="#state#">
    <option disabled value="">Select a city</option>
    <cfloop array="#getCities(state)#" index="city">
        <option value="#city.id#">#city.label#</option>
    </cfloop>
</select>
```
{% endtab %}
{% endtabs %}

### Multi-Select Dropdowns

```html
<select wire:model="states" multiple>
    <option value="AL">Alabama</option>
    <option value="AK">Alaska</option>
    <option value="AZ">Arizona</option>
</select>
```

{% hint style="info" %}
By default, `wire:model` only sends updates when actions are performed. Use `wire:model.live` for real-time validation or immediate server updates as users type.
{% endhint %}
