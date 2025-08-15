# wire:confirm

The `wire:confirm` directive adds a confirmation dialog before executing actions, protecting users from accidental clicks on destructive operations. When users click an element with `wire:confirm`, they'll see a browser confirmation dialog that must be accepted before the action proceeds.

## Basic Usage

This subscription management component demonstrates `wire:confirm` for protecting critical actions:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/SubscriptionManager.bx
class extends="cbwire.models.Component" {
    data = {
        "subscriptionActive": true,
        "userName": "John Doe"
    };

    function cancelSubscription() {
        data.subscriptionActive = false;
    }

    function deleteAccount() {
        // Permanently delete account logic
        redirect("/goodbye");
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/SubscriptionManager.cfc
component extends="cbwire.models.Component" {
    data = {
        "subscriptionActive" = true,
        "userName" = "John Doe"
    };

    function cancelSubscription() {
        data.subscriptionActive = false;
    }

    function deleteAccount() {
        // Permanently delete account logic
        redirect("/goodbye");
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/subscriptionManager.bxm -->
<bx:output>
<div>
    <h1>Account Settings</h1>
    
    <button wire:click="cancelSubscription" 
            wire:confirm="Are you sure you want to cancel your subscription?">
        Cancel Subscription
    </button>
    
    <button wire:click="deleteAccount" 
            wire:confirm.prompt="Please confirm deletion by typing 'DELETE' below|DELETE">
        Delete Account
    </button>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/subscriptionManager.cfm -->
<cfoutput>
<div>
    <h1>Account Settings</h1>
    
    <button wire:click="cancelSubscription" 
            wire:confirm="Are you sure you want to cancel your subscription?">
        Cancel Subscription
    </button>
    
    <button wire:click="deleteAccount" 
            wire:confirm.prompt="Please confirm deletion by typing 'DELETE' below|DELETE">
        Delete Account
    </button>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:confirm Does

When you add `wire:confirm` to an element, CBWIRE automatically:

- **Shows Confirmation Dialog**: Displays a browser confirmation dialog with your custom message
- **Blocks Action Execution**: Prevents the action from running if the user cancels
- **Continues on Accept**: Executes the action normally if the user confirms
- **Integrates with Actions**: Works seamlessly with any `wire:click` action

## Available Modifiers

The `wire:confirm` directive supports one modifier:

- **Prompt**: `.prompt` - Requires users to type a specific confirmation text

Example: `wire:confirm.prompt="Type DELETE to confirm|DELETE"`

{% hint style="info" %}
Use `wire:confirm` on any destructive or irreversible actions to prevent accidental data loss and improve user experience.
{% endhint %}