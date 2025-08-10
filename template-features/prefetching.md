---
description: Prefetch state changes when the user mouses over an HTML element. Fantastico!
---

# Prefetching

You can prefetch an [Action's](../essentials/actions.md) results on mouseOver using the `.prefetch` modifier.&#x20;

```xml
<div>
    <button wire:click.prefetch="togglePreview">Show Preview</button>

    <cfif args.showPreview>
        <!--- Preview goes here --->
    </cfif>
</div>
```

```javascript
component extends="cbwire.models.Component"{

    data = {
        "showPreview": false
    };

    function togglePreview(){
        data.showPreview = true;
    }
}
```

{% hint style="success" %}
In the example here, the `togglePreview` action will be prefetched and invoked when the user mouses over the button. The results of the fetch are not displayed until the user clicks the 'Show Preview' button.
{% endhint %}

{% hint style="warning" %}
Prefetching works well for actions that do not perform any side effects, such as mutating session data or writing to a database. If the action you are "pre-fetching" does have side effects, you may encounter unpredictable results.
{% endhint %}
