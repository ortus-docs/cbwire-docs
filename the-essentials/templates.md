# Templates

## Overview

Templates are your [components](components.md)' HTML and consist of valid HTML/CFML tags. This includes \<cfif>, \<cfloop>, etc.

{% tabs %}
{% tab title="BoxLang" %}
```html
<bx:output>
    <!--- HTML template goes here --->
    <div>
        <button wire:click="doSomething">Click here</button>
        <!-- Any valid CFML here -->
        <bx:if datePart( 'h', now() ) lt 12>
            <p>Good morning!</p>
        <bx:else>
            <p>Good afternoon!</p>
        </bx:if>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<cfoutput>
    <!--- HTML template goes here --->
    <div>
        <button wire:click="doSomething">Click here</button>
        <!-- Any valid CFML here -->
        <cfif datePart( 'h', now() ) lt 12>
            <p>Good morning!</p>
        <cfelse>
            <p>Good afternoon!</p>
        </cfif>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Your templates must have a single outer element for CBWIRE to bind to your component and update the DOM properly. This can be any valid HTML element. Below, we are using a div element.
{% endhint %}

Below is a template with one outer element ( good ) and another with two outer elements ( bad ).

```html
<!--- GOOD: single outer element --->
<div>
    <p>My awesome component</p>
</div>


<!--- BAD: 2 outer elements --->
<div>
    My awesome component    
</div>
<div>
    <p>This won't work.</p>
</div>
```

## Implicit Rendering

By default, CBWIRE will look in the **./wires** folder for a **.bxm or .cfm** file with the same name as your component.

```
./wires/Counter.bx <--- Boxlang class
./wires/Counter.cfc <--- CFML class (component)
./wires/Counter.bxm <--- Boxlang template file
./wires/Counter.cfm <--- CFML template file
```

{% hint style="info" %}
You can change this default location in the [configuration](../configuration.md) settings.
{% endhint %}

## Explicit Rendering

To override the default implicit rendering, we can tell CBWIRE where our component template is by defining a **onRender()** method on our component and using the **template()** method.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/MyTemplate.bx
class extends="cbwire.models.Component" {
    function onRender() {
        return template( "someFolder.MyTemplate" ); // renders ./somefolder/MyTemplate.bxm    
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/MyTemplate.cfc
component extends="cbwire.models.Component" {
    function onRender() {
        return template( "someFolder.MyTemplate" ); // renders ./somefolder/MyTemplate.cfm    
    }
}
```
{% endtab %}
{% endtabs %}

We can pass parameters when calling **template()**.

```javascript
function onRender() {
    return template( "someFolder.MyTemplate", { "someVar": "value" } );
}
```

In this case, **someVar** becomes available to our template.

```html
<!--- ./someFolder/MyTemplate.bxm|cfm --->
<cfoutput>
    <div>
        Somevar: #someVar#
    </div>
</cfoutput>
```

You can also return your template inline like so:

```javascript
function onRender() {
    return "<div>My component</div>";
}
```

## Using Data Properties

You can access your [data properties](templates.md#data-properties) from your template by calling **#propertyName#**.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Greeter.bx
class extends="cbwire.models.Component" {
    data = {
        "greeting": "Hello from CBWIRE"
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Greeter.cfc
component extends="cbwire.models.Component" {
    data = {
        "greeting": "Hello from CBWIRE"
    };
}
```
{% endtab %}
{% endtabs %}

```html
<!--- ./wires/greeter.bxm|cfm --->
<cfoutput>
    <div>
        Greeting: #greeting#
    </div>
</cfoutput>
```

## Using Computed Properties

You define [computed properties](templates.md#computed-properties) on your component by using the **computed** annotatio&#x6E;**.**&#x20;

```javascript
function greeting() computed {}
```

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Greeter.bx
class extends="cbwire.models.Component" {
    function greeting() computed {
        return "Hello from CBWIRE";
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Greeter.cfc
component extends="cbwire.models.Component" {
    function greeting() computed {
        return "Hello from CBWIRE";
    }
}
```
{% endtab %}
{% endtabs %}

You access computed properties in your template by invoking the method.

```html
<!--- ./wires/greeter.bxm|cfm --->
<div>
    Greeting: #greeting()#
</div>
```

{% hint style="info" %}
Computed properties are cached are are executed when first called. See [Computed Properties](computed-properties.md).
{% endhint %}

## Using Helper Methods

You can access any global helper methods defined in your ColdBox application and any modules you have installed.

For example, suppose you've installed the cbi8n module ( an internalization module for ColdBox ). You can access its global helper methods in your template, such as the **$r()** method for displaying text in various languages.

```html
<div>
    #$r( "greeting" )#
</div>
```

{% hint style="info" %}
Visit [ForgeBox](https://forgebox.io/) to find ColdBox modules for your application.
{% endhint %}
