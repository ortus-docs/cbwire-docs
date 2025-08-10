# Upgrading from CBWIRE 2.x

CBWIRE has dramatically improved since its v2.x days. Here, we will outline the core differences between CBWIRE 2 and CBWIRE 4 to assist you with upgrading.

## Overview

Here's a quick list of features in 4.x you'll get when upgrading:

* Easier access to data properties, component methods, and computed properties from your templates.
* Single-file components
  * Define your component in a single .CFM file
* Upgraded DOM diffing engine via Livewire.js
* Ability to pass keys with your wires to control when CBWIRE renders your components.
  * \#wire( name="MyForm", key="my-form" )#
* New HTML Directives
  * [wire:confirm](template-directives/wire-confirm.md)
  * [wire:navigate](template-directives/wire-navigate.md)
  * [wire:transition](template-directives/wire-transition.md)
  * [wire:stream](template-directives/wire-stream.md)
* [Alpine.js](upgrading-from-cbwire-2.x.md#alpine.js) is included
  * $wire object allows you to access and interact with your wire from JavaScript directly
* [Lazy loading](features/lazy-loading.md) with placeholders

## Layout

In 2.x, you had to place **wireStyles()** and **wireScripts()** in your layout like this to include CBWIRE's JavaScript and CSS assets.

```html
<!--- ./layouts/Main.cfm --->
<html>
    <head>
        #wireStyles()#
    </head>
    <body>
        <h1>CBWIRE Rocks</h1>
        #wireScripts()#
    </body>
</html>
```

**This is now automatically handled for you by default.**

If you still want to use these methods in your layout manually, you are welcome to do so, but you will need to update your ColdBox.cfc module settings. See [Configuration](configuration.md).

```javascript
// ./config/ColdBox.cfc
component{
    function configure() {
        moduleSettings = {
            "cbwire" = {
                "autoInjectAssets": false
            }
        };
     }
}
```

## Alpine.js

In 2.x, you had to include Alpine.js into your project via CDN or Webpak manually.

```html
<!--- ./layouts/Main.cfm --->
<html>
    <head>
        <script src="//unpkg.com/alpinejs" defer></script>
    </head>
    <body>
        ...
    </body>
</html>
```

**Alpine.js is now automatically included with CBWIRE 4.**&#x20;

CBWIRE and Alpine.js have such a deep integration together that automatically including the two makes sense, and this follows the direction that Livewire 3 took. There is no longer a need to pull Alpine.js into your project.&#x20;

{% hint style="warning" %}
Removing any previous installations or references to Alpine.js is important since it is now included with CBWIRE. Otherwise, you will run into various rendering errors, and your Alpine components will likely run twice.
{% endhint %}

## $wire

In 4.x, you can fully interact with your components using Alpine and the magical **$wire** object.

```html
<!--- Include our form in a view or layout --->
#wire( "Form" )#
```

```javascript
// ./wires/Form.cfc
component extends="cbwire.models.Component" {
    function submitForm() {
        // Do something important here
        sleep( 2000 );
    }
}
```

````html
<!--- ./wires/form.cfm --->
<div 
    x-data="{
        loading: false,
        async submitForm() {
            this.loading = true // show loader
            console.log( 'Submitting form...' )
            await $wire.submitForm() // Run our submitForm
            this.loading = false // hide loader
        }
    }">
    <h1>My Form</h1>
    
    <form @submit.prevent="submitForm">
        <button type="submit">Submit Form</button>
    </form>

    <template x-if="loading">
        <div>Show an awesome spinner here</div>
    </template>
</div>
```
````

See the [Alpine](the-essentials/javascript.md#alpine.js) section of these docs. Learn more about Alpine at [https://alpinejs.dev/](https://alpinejs.dev/).

## Single-file components

In 2.x, you defined your components with a .CFC file and .cfm template.&#x20;

You can still use separate files in 4.x, but you can also place everything in a single .cfm file. Use whichever pattern you prefer.

```html
<!--- ./wires/DayChecker.cfm --->
<cfoutput>
    <div>
        <cfif isMonday()>
            Bummer.
        </cfif>
    </div>
</cfoutput>

<cfscript>
    // @startWire
    function isMonday() computed {
        return false; // Let's skip Mondays    
    }
    // @endWire
</cfscript>
```

## Rendering Components

You still render your components using the **wire()** method, but the signature has changed.

In 2.x, the signature was:

```javascript
wire( required string componentName, struct parameters = {} )
```

In 4.x, the signature is:

```javascript
wire(
    required string name,
    struct params = {},
    string key = "",
    lazy = false,
    lazyIsolated = true 
)
```

Key differences:

* The arguments **componaneName** and **parameters** were given shorter names.
* You can provide a key, which is essential for correct rendering when nesting components ( See [Components](the-essentials/components.md) )
* You can lazy load components ( See [Lazy Loading](features/lazy-loading.md) )

## Data Properties

In 2.x, data properties were referenced in your templates using **#args.prop#**.

```javascript
// ./wires/SomeWire.cfc
component extends="cbwire.models.Component" {
    data = {
        "goodDay": true
    };
}
```

```html
<!--- ./views/wires/somewire.cfm --->
<cfoutput>
    <div>
        <cfif args.goodDay>
            Enjoy your day!
        <cfelse>
            Hey, it probably will get worse.
        </cfif>
    </div>
</cfoutput>
```

In 4.x, you can access the data property without args.

```html
<!--- ./views/wires/somewire.cfm --->
<cfoutput>
    <div>
        <cfif goodDay>
            Enjoy your day!
        <cfelse>
            Hey, it probably will get worse.
        </cfif>
    </div>
</cfoutput>
```

You can still reference your props using args for backward compatibility, but it's optional.

## Component Methods

Any methods you define in your component you can access from the template.

```javascript
// ./wires/SomeWire.cfc
component extends="cbwire.models.Component" {
    data = {};
    
    // Returns the current date
    function getTomorrow() {
        return now().add( "d", 1 );
    }
}
```

```html
<!--- ./wires/somewire.cfm --->
<cfoutput>
    <div>
        Tomorrow: #dateFormat( getTomorrow(), "mm/dd/yyyy" )#
    </div>
</cfoutput>
```

## Computed Properties

In 2.x, computed properties were defined with a computed struct in your component.

```javascript
component extends="cbwire.models.Component" {

    computed = {
        "isMonday" : function() {
            return false; // Let's skip Mondays
        }
    }

}
```

And you would access them in your templates like this:

```html
<cfoutput>
    <div>
        <cfif args.computed.isMonday()>
            Bummer.
        </cfif>
    </div>
</cfoutput>
```

In 4.x, we define computed properties like this:

```javascript
component extends="cbwire.models.Component" {
    function isMonday() computed {
        return false; // Let's skip Mondays    
    }
}
```

And we access them in our templates like this:

```html
<cfoutput>
    <div>
        <cfif isMonday()>
            Bummer.
        </cfif>
    </div>
</cfoutput>
```

See [Computed Properties](upgrading-from-cbwire-2.x.md#computed-properties).

## Events

In 2.x, you could communicate with your components using **emit()**.

```javascript
component extends="cbwire.models.Component" {
    function sendEmail() {
        emit( "email-sent" );
    }
}
```

In 4.x, this has been replaced with **dispatch()**.

```javascript
component extends="cbwire.models.Component" {
    function sendEmail() {
        dispatch( "email-sent" );
    }
}
```

You can also dispatch from JavaScript using **$wire.$dispatch** and **cbwire.dispatch()**.

## Adobe CF 2018

Official support for Adobe CF 2018 was dropped.

## Validation

In 2.x, you defined your validation constraints using **this.constraints**.

```javascript
component extends="cbwire.models.Component" {
    this.constraints = {
        "email": { required: true }
    };
}
```

In 4.x, you use **constraints** or **variables.constraints**.

```javascript
component extends="cbwire.models.Component" {
    constraints = {
        "email": { required: true }
    };
}
```

## Relocating

In 2.x, relocating was done with the **relocate()** method. It's been replaced with **redirect()**;

## TurboLinks

TurboLinks has been replaced with [wire:navigate](template-directives/wire-navigate.md). By adding a simple **wire:navigate** to your links, you can quickly load pages in the background and prevent re-rendering all your JS and CSS assets.

```html
<a href="/some/link" wire:navigate>Click here</a>
```

We no longer recommend using the Turbolinks library for SPA-style applications.

