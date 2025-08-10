---
description: >-
  CBWIRE is a ColdBox module that helps you build modern, reactive CFML
  applications in record time - without using much JavaScript or building
  backend APIs. Become a web development hero with CBWIRE!
---

# Introduction

<figure><img src=".gitbook/assets/logo (1).png" alt=""><figcaption></figcaption></figure>

## Your First Component

Download and install [CommandBox](https://www.ortussolutions.com/products/commandbox). From your CLI, start CommandBox by typing 'box'. Then run the following:

```
mkdir cbwire-playground --cd
coldbox create app
install cbwire@be
server start
```

Let's insert a counter element into our Main layout using **wire( "Counter" )**_._

```markup
<!--- ./layouts/Main.cfm --->
<cfoutput>
<!doctype html>
<html>
<head>
    <!--- CBWIRE CSS --->
    #wireStyles()#
</head>
<body>
    <!--- INSERT A COUNTER HERE --->
    #wire( "Counter" )#

    <!--- CBWIRE JS --->
    #wireScripts()#
</body>
</html>
</cfoutput>
```

Let's define our Counter component using the path **./wires/Counter.cfm**.&#x20;

```html
<cfscript>
    // Data properties
    data = {
        "counter": 0 // default value
    };
    
    // Actions
    function increment() {
        data.counter += 1;
    }
</cfscript>

<!--- Our template --->
<cfoutput>
    <div>
        <div>Count: #counter#</div>
        <button wire:click="increment">+</button>
    </div>
</cfoutput>
```

You now have a reactive counter that increments when you click the plus button without any page refreshing or writing JavaScript<mark style="color:blue;">!</mark> 🤯

![](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FEWvXvmxEDxwnAJzKYkxz%2Fuploads%2FPESERLoILEJOeO4yXCCn%2F2022-02-08%2016.02.25.gif?alt=media\&token=7846eb9)

## What!? How?

1. CBWIRE automatically processes the HTML template, data properties, and actions of our component, then displays the component with its initial settings (which begin at 0).
2. When a user clicks a button, CBWIRE initiates an XMLHTTPRequest (XHR) to communicate with the server.
3. CBWIRE captures this XMLHTTPRequest and triggers the 'increment()' action, leading to an update in our data property.
4. Following this, CBWIRE updates the HTML template and includes this revised HTML in the XMLHTTPRequest response.
5. CBWIRE monitors any state changes and employs JavaScript and DOM comparison techniques to refresh the display on the screen.

## Awesome, right?

* We developed a responsive counter using just one file: wires/Counter.cfm.
* We avoided writing any JavaScript code.
* We didn't need to create an API.
* There was no need for page refreshes.
* We skipped using webpack or dealing with JavaScript compilation.
* We stayed entirely within the CFML environment. :nerd:

CBWIRE is transforming the way we build CFML applications, and we think you're going to love it too!

## Credits

CBWIRE uses [Livewire](https://laravel-livewire.com/) for its client-side functionality and DOM diffing and wouldn't exist without the awesome work of [Caleb Porzio](https://twitter.com/calebporzio) ( creator of [Livewire](https://laravel-livewire.com/), [Alpine.js](https://github.com/alpinejs/alpine) ).

The CBWIRE module for ColdBox is written and maintained by [Grant Copley](https://twitter.com/grantcopley), [Luis Majano](https://twitter.com/lmajano), and [Ortus Solutions](https://www.ortussolutions.com/).

## Project Support

Please consider becoming one of our lovingly esteemed [Patreon supporters](https://www.patreon.com/ortussolutions).
