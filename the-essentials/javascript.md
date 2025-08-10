# JavaScript

## Overview

CBWIRE includes Alpine.js, and uses Livewire.js for all client-side functionality and DOM diffing. These tools give you complete control over client-side functionality while providing ways to interact with your component server-side.

## Alpine.js

Alpine.js is included with CBWIRE and was designed to work beautifully with your CBWIRE components.

Using Alpine.js is as easy as declaring an **x-data** in your [template](templates.md). You can declare client-side properties, actions, and interact with your component using the **$wire** object.

```html
<div
    x-data="{
        counter: 0,
        increment() {
            this.counter++   
        },
        async save() {
            // Call the save method on our component
            await $wire.save( this.counter );
        }
    }"
    wire:ignore.self>
    <div>Count: <span x-text="counter"></span></div>
    <button @click="increment">+</button>
    <button @click="save">Save to database</button>
</div>
```

{% hint style="info" %}
Learn more on the [Alpine.js](../features/alpine.js.md) features page.
{% endhint %}

## $wire object

{% hint style="success" %}
The **$wire** object is one of the most powerful features in Livewire.
{% endhint %}

With **$wire**, you get a JavaScript representation of your server-side CBWIRE component that you can interact with. You can change data properties, call actions, access parent components, fire events, and much more.

Below are the properties and methods of the **$wire** object:

```javascript
let $wire = {
    // All component public properties are directly accessible on $wire...
    counter: 0,
 
    // All public methods are exposed and callable on $wire...
    increment() { ... },
 
    // Access the `$wire` object of the parent component if one exists...
    $parent,
 
    // Access the root DOM element of the CBWIRE component...
    $el,
 
    // Access the ID of the current CBWIRE component...
    $id,
 
    // Get the value of a property by name...
    // Usage: $wire.$get('count')
    $get(name) { ... },
 
    // Set a property on the component by name...
    // Usage: $wire.$set('count', 5)
    $set(name, value, live = true) { ... },
 
    // Toggle the value of a boolean property...
    $toggle(name, live = true) { ... },
 
    // Call the method
    // Usage: $wire.$call('increment')
    $call(method, ...params) { ... },
 
    // Entangle the value of a CBWIRE property with a different,
    // arbitrary, Alpine property...
    // Usage: <div x-data="{ count: $wire.$entangle('count') }">
    $entangle(name, live = false) { ... },
 
    // Watch the value of a property for changes...
    // Usage: Alpine.$watch('counter', (value, old) => { ... })
    $watch(name, callback) { ... },
 
    // Refresh a component by sending a commit to the server
    // to re-render the HTML and swap it into the page...
    $refresh() { ... },
 
    // Identical to the above `$refresh`. Just a more technical name...
    $commit() { ... },
 
    // Listen for a an event dispatched from this component or its children...
    // Usage: $wire.$on('post-created', () => { ... })
    $on(event, callback) { ... },
 
    // Dispatch an event from this component...
    // Usage: $wire.$dispatch('post-created', { postId: 2 })
    $dispatch(event, params = {}) { ... },
 
    // Dispatch an event onto another component...
    // Usage: $wire.$dispatchTo('dashboard', 'post-created', { postId: 2 })
    $dispatchTo(otherComponentName, event, params = {}) { ... },
 
    // Dispatch an event onto this component and no others...
    $dispatchSelf(event, params = {}) { ... },
 
    // A JS API to upload a file directly to component
    // rather than through `wire:model`...
    $upload(
        name, // The property name
        file, // The File JavaScript object
        finish = () => { ... }, // Runs when the upload is finished...
        error = () => { ... }, // Runs if an error is triggered mid-upload...
        progress = (event) => { // Runs as the upload progresses...
            event.detail.progress // An integer from 1-100...
        },
    ) { ... },
 
    // API to upload multiple files at the same time...
    $uploadMultiple(name, files, finish, error, progress) { },
 
    // Remove an upload after it's been temporarily uploaded but not saved...
    $removeUpload(name, tmpFilename, finish, error) { ... },
 
    // Retrieve the underlying "component" object...
    __instance() { ... },
}
```

## Executing Scripts

You can execute bespoke JavaScript in your CBWIRE component using **\<cbwire:script>**.

<pre class="language-html"><code class="lang-html">&#x3C;!--- ./wires/mycomponent.bxm|cfm --->
&#x3C;div>...&#x3C;/div>

<strong>&#x3C;cbwire:script>
</strong>    &#x3C;script>
        // This Javascript will get executed every time this component is loaded onto the page...
    &#x3C;/script>
&#x3C;/cbwire:script>
</code></pre>

Here's a complete example:

```html
<!--- ./wires/counter.bxm|cfm --->
<div>
    Counter component in Alpine:
 
    <div x-data="counter">
        <h1 x-text="count"></h1>
        <button x-on:click="increment">+</button>
    </div>
</div>
 
<cbwire:script>
    <script>
        Alpine.data('counter', () => {
            return {
                count: 0,
                increment() {
                    this.count++
                },
            }
        })
    </script>
</cbwire:script>
```

{% hint style="info" %}
Scripts are executed after the page has loaded BUT before your CBWIRE component has rendered.
{% endhint %}

{% hint style="warning" %}
Unlike assets, which are loaded only once per page, scripts are evaluated for every component instance.
{% endhint %}

## Loading Assets

You can load style assets on the page with your component using **\<cbwire:assets>**.

Here's an example of loading a date picker called [Pikaday](https://github.com/Pikaday/Pikaday):

```html
<!--- ./wires/date.bxm|cfm --->
<div>
    <input type="text" data-picker>
</div>
 
<cbwire:assets>
    <script src="https://cdn.jsdelivr.net/npm/pikaday/pikaday.js" defer></script>
    <link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/npm/pikaday/css/pikaday.css">
</cbwire:assets>
 
<cbwire:script>
    <script>
        new Pikaday({ field: $wire.$el.querySelector('[data-picker]') });
    </script>
</cbwire:script>
```

{% hint style="info" %}
Livewire will ensure any assets are loaded on the page before evaluating scripts. It will also ensure that assets are only loaded once per page, even if you have multiple instances of the component.
{% endhint %}

## Using $wire From Scripts

You can access your component's **$wire** object within your scripts block.

```html
<cbwire:script>
    <script>
        setInterval( () => {
            $wire.$refresh()
        }, 3000 );
    </script>
</cbwire:script>
```

## Livewire Events

You can access the **livewire:init** and **livewire:initialized** events as Livewire is loading.

```html
<script>
    document.addEventListener('livewire:init', () => {
        // Runs after Livewire is loaded but before it's initialized
        // on the page...
    } );
 
    document.addEventListener('livewire:initialized', () => {
        // Runs immediately after Livewire has finished initializing
        // on the page...
    } );
</script>
```

## Livewire object

You can interact with Livewire from external scripts using the **Livewire** global object.

```javascript
// Retrieve the $wire object for the first component on the page...
let component = Livewire.first()
 
// Retrieve a given component's `$wire` object by its ID...
let component = Livewire.find(id);
 
// Retrieve an array of component `$wire` objects by name...
let components = Livewire.getByName(name);
 
// Retrieve $wire objects for every component on the page...
let components = Livewire.all();

// Dispatch an event to any CBWIRE components listening...
Livewire.dispatch( 'post-created', { postId: 2 } );
 
// Dispatch an event to a given CBWIRE component by name...
Livewire.dispatchTo( 'dashboard', 'post-created', { postId: 2 } );
 
// Listen for events dispatched from CBWIRE components...
Livewire.on('post-created', ( { postId } ) => {
    // ...
} );

// Register a callback to execute on a given internal Livewire hook...
Livewire.hook( 'component.init', ( { component, cleanup } ) => {
    // ...
})
```

## Custom Directives

Livewire allows you to register custom directives using **Livewire.directive()**.

Here's the implementation for [wire:confirm](../template-directives/wire-confirm.md):

```html
<button wire:confirm="Are you sure?" wire:click="delete">Delete post</button>
```

```javascript
Livewire.directive( 'confirm', ( { el, directive, component, cleanup } ) => {
    let content =  directive.expression;
 
    // The "directive" object gives you access to the parsed directive.
    // For example, here are its values for: wire:click.prevent="deletePost(1)"
    //
    // directive.raw = wire:click.prevent;
    // directive.value = "click";
    // directive.modifiers = ['prevent'];
    // directive.expression = "deletePost(1)";
 
    let onClick = e => {
        if (! confirm( content ) ) {
            e.preventDefault()
            e.stopImmediatePropagation()
        }
    }
 
    el.addEventListener( 'click', onClick, { capture: true } );
 
    // Register any cleanup code inside `cleanup()` in the case
    // where a Livewire component is removed from the DOM while
    // the page is still active.
    cleanup( () => {
        el.removeEventListener( 'click', onClick );
    } );
})
```

## JavaScript Hooks

```javascript
Livewire.hook('component.init', ({ component, cleanup }) => {
    // Fires every time a new component is discovered by Livewire
    // Both on page load and later on
})

Livewire.hook('element.init', ({ component, el }) => {
    // Fires for each DOM element within a given CBWIRE component
})

Livewire.hook('morph.updating',  ({ el, component, toEl, skip, childrenOnly }) => {
    // DOM morphing hook
})
 
Livewire.hook('morph.updated', ({ el, component }) => {
    // DOM morphing hook
})
 
Livewire.hook('morph.removing', ({ el, component, skip }) => {
    // DOM morphing hook
})
 
Livewire.hook('morph.removed', ({ el, component }) => {
    // DOM morphing hook
})
 
Livewire.hook('morph.adding',  ({ el, component }) => {
    // DOM morphing hook
})
 
Livewire.hook('morph.added',  ({ el }) => {
    // DOM morphing hook
})
```

## Intercepting Commits

A commit is made every time a CBWIRE component is sent to the server. You can hook into an individual commit like this:

```javascript
Livewire.hook('commit.prepare', ({ component, commit }) => {
    // Runs before commit payloads are collected and sent to the server...
})

Livewire.hook('commit', ({ component, commit, respond, succeed, fail }) => {
    // Runs immediately before a commit's payload is sent to the server...
 
    respond(() => {
        // Runs after a response is received but before it's processed...
    })
 
    succeed(({ snapshot, effect }) => {
        // Runs after a successful response is received and processed
        // with a new snapshot and list of effects...
    })
 
    fail(() => {
        // Runs if some part of the request failed...
    })
})
```

## Request Hooks

Using the **request** hook, you can hook into the entire HTTP request, which may contain multiple commits.

```javascript
Livewire.hook('request', ({ uri, options, payload, respond, succeed, fail }) => {
    // Runs after commit payloads are compiled, but before a network request is sent...
 
    respond(({ status, response }) => {
        // Runs when the response is received...
        // "response" is the raw HTTP response object
        // before await response.text() is run...
    })
 
    succeed(({ status, json }) => {
        // Runs when the response is received...
        // "json" is the JSON response object...
    })
 
    fail(({ status, content, preventDefault }) => {
        // Runs when the response has an error status code...
        // "preventDefault" allows you to disable Livewire's
        // default error handling...
        // "content" is the raw response content...
    })
})
```

## Customizing Page Expiration

If the default page expiration dialog isn't suitable for your application, you can use the request hook to implement a custom solution.

```html
<script>
    document.addEventListener('livewire:init', () => {
        Livewire.hook('request', ({ fail }) => {
            fail(({ status, preventDefault }) => {
                if (status === 419) {
                    confirm('Your custom page expiration behavior...')
                    preventDefault()
                }
            })
        })
    })
</script>
```

