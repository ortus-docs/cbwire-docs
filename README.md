---
description: >-
  CBWIRE is a ColdBox module that makes building modern, reactive CFML apps a
  breeze without the need for JavaScript frameworks such as Vue or React, and
  without the hassle of creating unnecessary APIs.
---

# Introduction



<figure><img src=".gitbook/assets/logo (1).png" alt=""><figcaption></figcaption></figure>

Are you tired of the challenges that come with building modern CFML apps? [ColdBox](https://coldbox.ortusbooks.com/) makes server-side app development a breeze, but sometimes the client-side is a whole different story. With powerful JavaScript frameworks like Vue and React, it can be a difficult and time-consuming process to create your web apps.&#x20;

But what if you could have the best of both worlds: the power of Vue and React with the simplicity of CFML? Impossible, you say? Think again!&#x20;

Introducing **CBWIRE: Power up your CFML and make your app dreams a reality!**

## Let's create a counter...

Install [CommandBox](https://www.ortussolutions.com/products/commandbox), then from our terminal, run:

```bash
mkdir cbwire-demo
cd cbwire-demo
box coldbox create app
box install cbwire@be
box server start
```

Next, add `wireStyles()` and `wireScripts()` to our layout. This is required for CBWIRE to do it's magic.&#x20;

Let's also insert a counter element into the page using `wire( "Counter" )`.

```markup
<!--- ./layouts/Main.cfm --->
<cfoutput>
<!doctype html>
<html>
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

Let's define our counter [Wire](essentials/creating-components.md).&#x20;

```javascript
// ./wires/Counter.cfc
component extends="cbwire.models.Component" {

    // Data Properties
    data = {
        "counter": 0
    };

    // Actions
    function increment(){
        data.counter += 1;
    }
}

```

Finally, let's create a [Template](essentials/templates.md) for our counter Wire. Notice that we've added a `wire:click` [directive](template-features/directives.md) to our button.

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

Refresh the page. You now have a reactive counter that increments when you click the plus button without any page refreshing!

![](<.gitbook/assets/2022-02-08 16.02.25.gif>)

## What!? How?

1. CBWIRE renders our _Counter_ Wire template. Our counter value defaults to 0.
2. When a user clicks the plus button, CBWIRE detects the event and makes an XHR request to the server.
3. CBWIRE picks up the XHR request and invokes the _increment_ action.
4. The _increment_ method updates the counter state by 1.
5. CBWIRE re-renders the template and returns the updated HTML in the XHR response
6. CBWIRE detects any state changes and uses Livewire to mutate the DOM.

## Awesome, right?

* We built a reactive counter with no page refreshing.
* We didn't write any JavaScript.
* We didn't use webpack or mess with JavaScript compilation.&#x20;
* We never left CFML.:nerd:&#x20;

CBWIRE is transforming the way we build CFML applications, and we think you're going to love it also!

## Credits

CBWIRE is built on [Livewire](https://laravel-livewire.com/) and wouldn't exist without [Caleb Porzio](https://twitter.com/calebporzio) ( creator of [Livewire](https://laravel-livewire.com/), [Alpine.js](https://github.com/alpinejs/alpine) ) and the PHP community.&#x20;

The CBWIRE module for ColdBox is written and maintained by [Grant Copley](https://twitter.com/grantcopley), [Luis Majano](https://twitter.com/lmajano), and [Ortus Solutions](https://www.ortussolutions.com/).

## Project Support

Please consider becoming one of our lovingly esteemed [Patreon supporters](https://www.patreon.com/ortussolutions).

## Resources

* CBWIRE Examples: [https://github.com/grantcopley/cbwire-examples](https://github.com/grantcopley/cbwire-examples)
* ForgeBox: [https://forgebox.io/view/cbwire](https://forgebox.io/view/cbwire)
* GitHub Repository: [https://github.com/coldbox-modules/cbwire](https://github.com/coldbox-modules/cbwire)
* API Docs: [https://apidocs.ortussolutions.com/#/coldbox-modules/cbwire/](https://apidocs.ortussolutions.com/#/coldbox-modules/cbwire/)
* Issue Tracker: [https://github.com/coldbox-modules/cbwire/issues](https://github.com/coldbox-modules/cbwire/issues)
* Task List Demo: [https://github.com/grantcopley/cbwire-task-list-demo](https://github.com/grantcopley/cbwire-task-list-demo)
* Form Validation Demo: [https://github.com/grantcopley/cbwire-signup-form-demo](https://github.com/grantcopley/cbwire-signup-form-demo)
* Up and Running Screencast: [https://cfcasts.com/series/ortus-single-video-series/videos/up-and-running-with-cbwire](https://cfcasts.com/series/ortus-single-video-series/videos/up-and-running-with-cbwire)
* Into The Box 2021 Presentation: [https://cfcasts.com/series/into-the-box-2021/videos/cbwire-coldbox-+-livewire-grant-copley](https://cfcasts.com/series/into-the-box-2021/videos/cbwire-coldbox-+-livewire-grant-copley)
* Ortus Webinar 2022: [https://cfcasts.com/series/ortus-webinars-2022/videos/grant-copley-on-cbwire-+-alpine\_js](https://cfcasts.com/series/ortus-webinars-2022/videos/grant-copley-on-cbwire-+-alpine_js)
