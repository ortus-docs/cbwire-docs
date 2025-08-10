---
description: >-
  cbwire is a ColdBox module that makes building reactive, modern apps easy
  without leaving the comfort of CFML.
---

# Introduction

![](.gitbook/assets/cbWire300.png)

Building modern apps is complicated. [ColdBox](https://coldbox.ortusbooks.com) makes creating server-side CFML apps easy, but what about the client-side? Front-end JavaScript frameworks like Vue and React are powerful, yet they also introduce complexity and a significant learning curve when creating our apps.

What if you could create apps that look and feel like your Vue and React web apps... but are written with CFML? Impossible, you say? Nay, we say!

Introducing \*\*cbwire: Power-up your CFML!

## Let's create a Counter in cbwire...

Install [CommandBox](https://www.ortussolutions.com/products/commandbox), then from your terminal, run:

```bash
mkdir cbwire-demo
cd cbwire-demo
box coldbox create app .
box install cbwire@be
box server start
```

Let's add cbwire styling and script references to our layout using `wireStyles()` and `wireScripts()`, and include a cbwire component we will create using `wire( "Counter ")`.

```markup
<!--- ./layouts/Main.cfm --->
<cfoutput>
<!doctype html>
<html lang="en">
<head>
    #wireStyles()#
</head>
<body>
    #wire( "Counter" )#
    #wireScripts()#
</body>
</html>
</cfoutput>
```

Let's create our _Counter_ cbwire component.

```javascript
// File: ./wires/Counter.cfc
component extends="cbwire.models.Component" {

    // Reactive Data Properties
    variables.data = {
        "counter": "0"
    };

    // Action to increment counter
    function increment(){
        variables.data.counter += 1;
    }
}
```

```markup
<!--- ./views/wires/counter.cfm --->
<cfoutput>
<div>
    <div>Count: #args.counter#</div>
    <button wire:click="increment">
        +
    </button>
</div>
</cfoutput>
```

Now that you've created your _Counter_ component, you can include this component in any layout or view throughout your ColdBox app using the `wire()` helper method.

Refresh the page and you find a reactive counter that increments when you hit the plus button.

![](<.gitbook/assets/image (2).png>)

## How is this working!!!?

1. cbwire renders our _Counter_ component with our `.cfm` page. ( View Source to see this ). This means cbwire is SEO-friendly.
2. When a user clicks the plus button, cbwire makes an AJAX request to the server and triggers the _increment_ action.
3. cbwire updates the counter state
4. cbwire re-renders the component template and returns the updated HTML in the AJAX response
5. cbwire is using the amazing [Livewire](https://laravel-livewire.com) JavaScript library to mutate the DOM based on our state changes.

## Pretty cool, right?

* We built a reactive counter.
* The counter updates without any page refresh.
* We didn't write any JavaScript.
* We didn't mess with webpack or JavaScript compilation.
* We never left CFML.:nerd:

We're just getting warmed up! cbwire is transforming the way we build CFML applications, and we think you're going to love it also.

## Credits

cbwire is built on [Livewire](https://laravel-livewire.com) and wouldn't exist if it wasn't for the efforts of Caleb Porzio ( creator of [Livewire](https://laravel-livewire.com), [Alpine.js](https://github.com/alpinejs/alpine) ) and the PHP community.

The cbwire module for ColdBox is written and maintained by [Grant Copley](https://twitter.com/grantcopley), [Luis Majano](https://twitter.com/lmajano), and [Ortus Solutions](https://www.ortussolutions.com).

## Project Support

Please consider becoming one of our lovingly esteemed [Patreon supporters](https://www.patreon.com/ortussolutions).
