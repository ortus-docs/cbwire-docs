---
description: Prefetch component updates as the user mouses over HTML elements.
---

# Prefetching

CBWIRE has a feature called prefetching which will prefetch the rendering of an action and store the HTML client-side without rendering the update to the DOM until the action is performed.&#x20;

Consider the example below. Notice we have added a _.prefetch_ modifier to our _wire:click_ directive.

```html
<button wire:click.prefetch="togglePreview">Show Preview</button>
```

```xml
<!--- ./wires/ShowImage.cfm --->
<cfoutput>
<div>
    <button wire:click.prefetch="togglePreview">Show Preview</button>

    <cfif showPreview>
        <!--- Preview goes here --->
    </cfif>
</div>
</cfoutput>

<cfscript>
    data = {
        "showPreview": false
    };

    function togglePreview(){
        data.showPreview = true;
    }
</cfscript>
```

{% hint style="info" %}
In the example here, the _togglePreview_ action will be prefetched and invoked when the user mouses over the button. The results of the fetch are not displayed until the user clicks the 'Show Preview' button.
{% endhint %}

{% hint style="warning" %}
Prefetching works well for actions that do not perform any side effects, such as mutating session data or writing to a database. If the action you are "pre-fetching" does have side effects, you may encounter unpredictable results.
{% endhint %}
