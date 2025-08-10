# wire:navigate

## Overview

You can use Livewire's **wire:navigate** feature to speed up page navigation and give your users a SPA-like experience.

```html
<nav>
    <a href="/" wire:navigate>Dashboard</a>
    <a href="/posts" wire:navigate>Posts</a>
    <a href="/users" wire:navigate>Users</a>
</nav>
```

When any link with **wire:navigate** is clicked, Livewire intercepts the click. Instead of allowing the browser to perform a full page visit, Livewire fetches the page in the background and swaps it with the current page. This results in much faster and smoother page navigation.

Here's a breakdown of what happens:

* The user clicks a link
* Livewire prevents the browser from visiting the new page
* Livewire requests the page in the background and shows a loading bar at the top of the page
* Once Livewire receives the HTML for the new page, it replaces the current URL, \<title> tag, and \<body> contents.

{% hint style="success" %}
This technique results in much faster page load times, often twice as fast, and makes your application feel like a JavaScript-powered single-page application.
{% endhint %}

## Prefetching on hover

You can add the **.hover** modifier to make Livewire prefetch a page when the user hovers over a link. This will ensure that the page has already been downloaded from the server when the user clicks on the link, making the contents load faster in the browser.

```html
<a href="/" wire:navigate.hover>Dashboard</a>
```

{% hint style="info" %}
The **.hover** modifier instructs Livewire to prefetch the page after a user has hovered over the link for 60 milliseconds.
{% endhint %}

{% hint style="warning" %}
Prefetching on hover increases server usage.
{% endhint %}

## Redirecting

You can instruct Livewire to use its **wire:navigate** functionality to load the new page when [redirecting](wire-navigate.md#redirecting) using CBWIRE's **redirect()** method by passing **redirectUsingNavigate** as **true**.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Form.bx
class extends="cbwire.models.Component" {
    function submit() {
        redirect( redirectURL="/thank-you", redirectUsingNavigate=true );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Form.cfc
component extends="cbwire.models.Component" {
    function submit() {
        redirect( redirectURL="/thank-you", redirectUsingNavigate=true );
    }
}
```
{% endtab %}
{% endtabs %}

## Persisting Elements Across Pages

Sometimes, you need to persist parts of a user interface between page loads, such as audio or video players.

You can achieve this using Alpine's [**x-persist**](https://alpinejs.dev/plugins/persist) plugin, which is included with CBWIRE.&#x20;

Here is an example of an audio player being persisted across pages.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./layouts/Main.bxm --->
<bx:output>
<html>
<body>
    ...
    <div x-persist="player">
        <audio src="#episode.file#" controls></audio>
    </div>
</body>
</html>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./layouts/Main.cfm --->
<cfoutput>
<html>
<body>
    ...
    <div x-persist="player">
        <audio src="#episode.file#" controls></audio>
    </div>
</body>
</html>
</cfoutput>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Persisted elements must be placed outside your CBWIRE [components](../the-essentials/components.md). A common practice is to put these in your main layout.
{% endhint %}

## Preserving Scroll Position

By default, Livewire preserves the scroll position of a page when navigating between pages. However, there may be times when you want to preserve the scroll position of an individual element between page loads.

To do this, you must add Livewire's **wire:scroll** to the element containing a scrollbar.

```html
<div x-persist="scrollbar">
    <div class="overflow-y-scroll" wire:scroll>
        ....
    </div>
</div>
```

## JavaScript Hooks

Each page navigation triggers three lifecycle hooks.

* livewire:navigate
* livewire:navigating
* livewire:navigated

```javascript
document.addEventListener('livewire:navigate', (event) => {
    // Triggers when a navigation is triggered.
 
    // Can be "cancelled" (prevent the navigate from actually being performed):
    event.preventDefault()
 
    // Contains helpful context about the navigation trigger:
    let context = event.detail
 
    // A URL object of the intended destination of the navigation...
    context.url
 
    // A boolean [true/false] indicating whether or not this navigation
    // was triggered by a back/forward (history state) navigation...
    context.history
 
    // A boolean [true/false] indicating whether or not there is
    // cached version of this page to be used instead of
    // fetching a new one via a network round-trip...
    context.cached
})
 
document.addEventListener('livewire:navigating', () => {
    // Triggered when new HTML is about to swapped onto the page...
 
    // This is a good place to mutate any HTML before the page
    // is nagivated away from...
})
 
document.addEventListener('livewire:navigated', () => {
    // Triggered as the final step of any page navigation...
 
    // Also triggered on page-load instead of "DOMContentLoaded"...
})
```

{% hint style="info" %}
These three hooks events are dispatched on navigations of all types. This includes manual navigation using **Livewire.navigate()**, redirecting with navigation enabled, and pressing the back and forward buttons in the browser.
{% endhint %}

## Event Listeners

{% hint style="warning" %}
Attaching an event listener to the document will not be removed when you navigate to a different page. This can lead to unexpected behavior if you need code to run only after navigating to a specific page.&#x20;
{% endhint %}

You can remove an event listener after it runs by passing the option **{once: true}** as a third parameter to the **addEventListener** function.

```javascript
document.addEventListener('livewire:navigated', () => {
    // ...
}, { once: true })
```

## Manually visiting a new page

You can use **Livewire.navigation()** to trigger a visit to a new page using JavaScript.

```html
<script>
    Livewire.navigate( '/new/url' );
</script>
```

## Using Analytics Software

When navigating pages using **wire:navigate,** any **\<script>** tags in the **\<head>** are evaluated once when the page is initially loaded.&#x20;

This creates a problem for some analytics software as these tools rely on a **\<script>** snippet evaluated on every page change, not just the first.

You can ensure your script tag is tracked properly using **data-spa="auto"**.

```html
<script src="https://cdn.usefathom.com/script.js" data-site="ABCDEFG" data-spa="auto" defer></script> 
```

## Caveats

When navigating to a new page using **wire:navigate**, it feels like the browser has changed pages. However, from the browser's perspective, you are technically still on the original page.

Here are a few caveats to consider.

{% hint style="info" %}
Don't rely on **DOMContentLoaded**. Use **livewire:navigated** instead. This will run code on every page visit.

```javascript
document.addEventListener( 'livewire:navigated', () => {
    // ...
} );
```
{% endhint %}

{% hint style="info" %}
Scripts in **\<head>** are loaded once. New **\<head>** scripts are evaluated.
{% endhint %}

{% hint style="info" %}
Add **data-navigate-track** to script tags in **\<head>** that include a version hash for cache busting. This will ensure Livewire detects asset changes and triggers a full page reload.

```html
<head>
    <script src="/app.js?id=123" data-navigate-track></script>
</head>
```
{% endhint %}

{% hint style="warning" %}
Scripts in **\<body>** are re-evaluated on each page load. You can use **data-navigate-once** to tell Livewire only to evaluate once.

```html
<script data-navigate-once>
    console.log('Runs only on first evaluation')
</script>
```
{% endhint %}

{% hint style="warning" %}
Head assets are blocking. When you navigate to a new page that contains assets like **\<script src="...">** in the head tag, those will be fetched and processed before the navigation is complete and the new page is swapped in.
{% endhint %}

## Customizing Progress Bar

Livewire will show a progress bar at the top of the page when a page takes longer than 150ms to load. You can customize the bar's color or disable it altogether in your [configuration](../configuration.md).&#x20;

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./config/ColdBox.bx
class {
    function configure() {
        moduleSettings = {
            "cbwire" : {
                "showProgressBar": true,
                "progressBarColor": "##2299dd"
            }
        };
     }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./config/ColdBox.cfc
component {
    function configure() {
        moduleSettings = {
            "cbwire" = {
                "showProgressBar": true,
                "progressBarColor": "##2299dd"
            }
        };
     }
}
```
{% endtab %}
{% endtabs %}

## Custom Loader Example

Using the available JavaScript hooks, you can create your own custom progress loader. Here's an example you could place in your layout file to show a custom loader.

```html
<!--- CBWIRE loader override --->
<script>
    document.addEventListener( 'livewire:navigate', () => {
        if ( !window.navigating ) {
            // Set flag to prevent multiple navigations
            window.navigating = true;
    
            // Prevent the navigate from actually being performed
            event.preventDefault();
    
            // Get target context from the navigation trigger
            let target = event.detail;
    
            // Show bootstrap loader (spinner)
            let loader = document.createElement( 'div' );
            loader.className = 'spinner-border text-primary';
            loader.setAttribute( 'role', 'status' );
            loader.innerHTML = '<span class="visually-hidden">Loading...</span>';
            document.getElementById( 'content' ).appendChild( loader );
    
            Livewire.navigate( target.url.pathname );
        }
    } , { once: true } );
    
    document.addEventListener( 'livewire:navigated', () => {
        window.navigating = false;
    } );
</script>
```
