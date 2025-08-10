# Rendering Views

### renderIt()&#x20;

You can set your component's template by defining a `renderIt()` method on your component.

```javascript
component extends="cbwire.models.Component" {

    function renderIt(){
        // Renders ./views/wires/myView.cfm
        return this.renderView( "myView" );
    }

}
```

### variables.view

You can also set your component template using `variables.view` as an alternative to renderIt().

```javascript
component extends="cbwire.models.Component" {
    variables.view = "myView";
}

```

### Implicit Lookup

If you haven't set your component's view using `renderIt()` or `variables.view`, cbwire will look for a file that matches your component's file name.

```javascript
// File: ./wires/Counter.cfc
component extends="cbwire.models.Component" {
    // No explicit view defined.
}
```

For the `Counter.cfc` example above, cbwire will attempt to render a view located under **./views/wires/counter.cfm**

{% hint style="info" %}
File names should be lowercase when using Implicit Lookups. This is to avoid issues on case-sensitive file systems.
{% endhint %}

### View Example

```xml
// File: ./views/wires/counter.cfm
<!--- GOOD --->
<cfoutput>
    <div>
        <button wire:click="increment">Increment Counter</button>
    </div>
</cfoutput>

<!--- BAD --->
<cfoutput>
    <button wire:click="increment">Does Not Work</button>
</cfoutput>
```

{% hint style="warning" %}
Be sure that cbwire can bind onto your component's view by including an outer element such as`<div>`. If an outer element is not detected, cbwire will throw an `OuterElementNotFound`exception.
{% endhint %}
