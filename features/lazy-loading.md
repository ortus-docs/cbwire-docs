# Lazy Loading

A slow component can slow down the loading of an entire page. CBWIRE's lazy loading feature allows you to delay loading your components until the page is fully rendered.&#x20;

Let's create a slow component that users will hate.

```html
<!--- ./views/someview.bxm|cfm --->
<div>
    #wire( "SlowComponent" )#
</div>
```

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/SlowComponent.bx
class extends="cbwire.model.Component" {
    function onMount() {
        sleep( 2000 ); // Make this component slow
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/SlowComponent.cfc
component extends="cbwire.model.Component" {
    function onMount() {
        sleep( 2000 ); // Make this component slow
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/slowcomponent.bxm --->
<bx:output>
    <div>
        <h1>Slow Component</h1>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/slowcomponent.cfm --->
<cfoutput>
    <div>
        <h1>Slow Component</h1>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

Changing this to a lazy-loaded component is as simple as using **wire( lazy=true )**.

```html
#wire( name="SlowComponent", lazy=true )#
```

This will cause Livewire to skip this component on the initial page load. Once visible in the viewport, Livewire will request a network to load the component fully.

{% hint style="info" %}
Lazy loading also delays the execution of the **onMount()** lifecycle method.
{% endhint %}

## Placeholder

As your components load, you often want to display some indicator, such as a spinner, to your users. You can do this easily using the **placeholder()** method.

```javascript
function onMount() {
    sleep( 2000 ); // Make this component slow
}

function placeholder() {
    return "<div>Loading...</div>";
}
```

{% hint style="info" %}
By default, CBWIRE inserts an empty **\<div>\</div>** for your component before it loads.
{% endhint %}

{% hint style="warning" %}
The placeholder and the main component must use the same root element type, such as a div. You will get rendering errors if you don't use the same root element.
{% endhint %}

## Placeholder using ColdBox view

You can render a ColdBox view for a placeholder using the **view()** method.

```javascript
function placeholder() {
    return view( "spinner" ); // renders ./views/spinner.bxm|cfm
}
```

