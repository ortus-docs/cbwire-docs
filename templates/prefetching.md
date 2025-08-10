# Prefetching

You can prefetch an action's results on mouseOver using the `.prefetch` modifier.&#x20;

```xml
// File: ./views/wires/movie.cfm
<div>
    <button wire:click.prefetch="togglePreview">Show Preview</button>

    <cfif args.showPreview>
        <!-- Preview goes here -->
    </cfif>
</div>
```

```javascript
// File: ./wires/Movie.cfc
component extends="cbwire.models.Component"{

    variables.data = {
        "showPreview": false
    };

    function togglePreview(){
        variables.data.contentIsVisible = true;
    }
    
    function renderIt(){
        return this.renderView( "wires/movie" );
    }
}
```

{% hint style="success" %}
In the example here, cbwire will invoke ( prefetch ) the `togglePreview` action when the user mouses over the button. The results of the fetch are not displayed until the user clicks the 'Show Preview' button.
{% endhint %}

{% hint style="warning" %}
Prefetching works well for actions that do not perform any side effects, such as mutating session data or writing to a database. If the action you are "pre-fetching" does have side effects, you may encounter unpredictable results.
{% endhint %}
