# wire:confirm

You can use **wire:confirm** in your [templates](../the-essentials/templates.md) to prompt users for confirmation before executing actions. This can be useful when dealing with potentially irreversible actions such as deletions or updates.

```html
<div>
    <button type="button" wire:click="cancelSubscription" wire:confirm="Are you sure you want to cancel your subscription?">
        Cancel Subscription
    </button>
</div>
```

## Prompting for input

You can add a **.prompt** modifier if you want to require an extra layer of confirmation.

```html
<div>
    <button type="button"
        wire:click="cancelSubscription"
        wire:confirm.prompt="Please confirm cancellation by typing 'CANCEL' below|CANCEL">
        Cancel Subscription
    </button>
</div>
```
