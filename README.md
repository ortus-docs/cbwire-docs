---
description: >-
  cbwire is a drop-in module for ColdBox that makes building dynamic/reactive
  interfaces incredibly easy, without leaving the comfort of ColdBox and CFML.
---

# Introduction

## cbwire - v1.0

**There's so much to building modern web apps these days.** [ColdBox](https://coldbox.ortusbooks.com/) makes things easy for us on the server-side, but what about our front-end development? ****JavaScript frameworks such as Vue and React are incredibly powerful and have changed the way most of us build web apps, but they've also introduced a great deal of complexity, as well as a significant learning curve when creating our applications.

**What if there was a way to build dynamic web applications that look and feel just like your Vue or React web apps, with little to no JavaScript, and while never leaving CFML?**

Impossible you say? **Nay** we say! Introducing **cbwire : Reactivate your CFML!**

## A real-time search component built with cbwire in minutes...

```javascript
component
    extends="cbwire.models.Component"
    accessors="true"
{

    // Wirebox injection ( wee! )
    property
        name="userService"
        inject="UserService@MyModule";

    // Local property that hold's the search typed into the UI
    property
        name="search"
        default="";

    // Render our view
    function $renderIt(){
        return this.$renderView(
            "wires/searchUsers",
            { "users" : userService.findBySearch( getSearch() ) }
        );
    }

}

```

```markup
// File: ./views/wires/searchUsers.cfm

<cfoutput>
<div>
    <input 
        wire:model="search" 
        type="text" 
        placeholder="Search users..."
    />

    <ul>
        <cfloop array="#args.users#" index="user">
            <li>#user.getUsername()#</li>
        </cfloop>
    </ul>
</div>
</cfoutput>
```

Now that you've defined your _cbwire_ component and view, you can include this anywhere in your app using our `wire()` helper method.

```markup
// File ./layouts/main.cfm

...
<cfoutput>
<body>
    <!-- Renders our search users form and updates in real-time as user types -->
    #wire( "SearchUsers" )#
</body>
</cfoutput>
...
```

Let's take a moment to reflect on what in the world just happened.

1. _cbwire_ renders the initial component out with our `.cfm` page, which means it's SEO friendly.
2. When a user types into the search, _cbwire_ makes an AJAX request to the server with the updated state.
3. The server re-renders the component and responds with the new HTML.
4. _cbwire_ utilitizes the amazing [_Livewire_](https://laravel-livewire.com/) JavaScript library to intelligently mutate the DOM based on our state changes.

More reflecting...

* We built a real-time search in a matter of just a few minutes.
* We didn't use any JavaScript.
* We didn't have to worry about _webpack_ configuration or compilation. 
* We didn't create an API endpoint or worry about any of the complexities that an API introduces.
* We never left CFML.
* We used awesome baked-in _ColdBox_ features, such as view rendering and dependency injection w/ [_WireBox_](https://wirebox.ortusbooks.com/).

## Unreal, right?

We're just getting warmed up! Browse through the other sections of this documentation to see just how much _cbwire_ can transform your applications and improve your development experience. This has changed the way we are building apps, and we think you're going to love it also.

## Credits

_cbwire_ wouldn't even exist if it wasn't for the awesome efforts of Caleb Porzio \( creator of [_Livewire_](https://laravel-livewire.com/), also [_Alpine.js_](https://github.com/alpinejs/alpine) \) and the PHP Laravel community. Livewire was originally created for use with the PHP framework Laravel. _cbwire_ is a port of the existing functionality over to CFML and the ColdBox framework.

The _cbwire_ module for ColdBox is written and maintained by Grant Copley, Luis Majano, and [Ortus Solutions](https://www.ortussolutions.com/).

