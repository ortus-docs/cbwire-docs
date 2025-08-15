# wire:navigate

The `wire:navigate` directive provides SPA-like page navigation by intercepting link clicks and loading pages in the background. Instead of full page refreshes, Livewire fetches new content and swaps it seamlessly, resulting in faster navigation and a smoother user experience.

## Basic Usage

This navigation example demonstrates `wire:navigate` with hover prefetching and redirect functionality:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/BlogPost.bx
class extends="cbwire.models.Component" {
    data = {
        "title": "",
        "content": ""
    };

    function save() {
        // Save blog post
        redirect("/blog", true); // Navigate using wire:navigate
    }

    function cancel() {
        redirect("/blog", true);
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/BlogPost.cfc
component extends="cbwire.models.Component" {
    data = {
        "title" = "",
        "content" = ""
    };

    function save() {
        // Save blog post
        redirect("/blog", true); // Navigate using wire:navigate
    }

    function cancel() {
        redirect("/blog", true);
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- layouts/Main.bxm -->
<bx:output>
<nav>
    <a href="/" wire:navigate>Dashboard</a>
    <a href="/blog" wire:navigate.hover>Blog</a>
    <a href="/users" wire:navigate>Users</a>
</nav>

<!-- Persisted audio player -->
<div x-persist="player">
    <audio src="#episode.file#" controls></audio>
</div>

<main>
    #wire("BlogPost")#
</main>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- layouts/Main.cfm -->
<cfoutput>
<nav>
    <a href="/" wire:navigate>Dashboard</a>
    <a href="/blog" wire:navigate.hover>Blog</a>
    <a href="/users" wire:navigate>Users</a>
</nav>

<!-- Persisted audio player -->
<div x-persist="player">
    <audio src="#episode.file#" controls></audio>
</div>

<main>
    #wire("BlogPost")#
</main>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:navigate Does

When you add `wire:navigate` to a link, CBWIRE automatically:

- **Intercepts Clicks**: Prevents browser from performing full page visits
- **Background Fetching**: Requests new page content via AJAX with loading indicator
- **Seamless Swapping**: Replaces URL, `<title>`, and `<body>` content without refresh
- **Performance Boost**: Often achieves 2x faster page loads than traditional navigation

## Available Modifiers

The `wire:navigate` directive supports one modifier:

- **Hover**: `.hover` - Prefetches page content when user hovers over link for 60ms

Example: `wire:navigate.hover` improves perceived performance but increases server usage.

## Navigation Process

Here's what happens during navigation:

1. User clicks a `wire:navigate` link
2. Livewire prevents default browser navigation
3. Loading bar appears at top of page
4. Page content is fetched in background
5. New content replaces current page seamlessly

## Redirecting with Navigation

Use CBWIRE's `redirect()` method with navigation enabled:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
function submit() {
    redirect("/thank-you", true); // Second parameter enables wire:navigate
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
function submit() {
    redirect("/thank-you", true); // Second parameter enables wire:navigate
}
```
{% endtab %}
{% endtabs %}

## Persisting Elements Across Pages

Use Alpine's `x-persist` to maintain elements like audio/video players:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- layouts/Main.bxm -->
<bx:output>
<div x-persist="player">
    <audio src="#episode.file#" controls></audio>
</div>

<div x-persist="chat">
    <div class="chat-widget">Live chat...</div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- layouts/Main.cfm -->
<cfoutput>
<div x-persist="player">
    <audio src="#episode.file#" controls></audio>
</div>

<div x-persist="chat">
    <div class="chat-widget">Live chat...</div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Preserving Scroll Position

Livewire preserves page scroll by default. For specific elements, use `wire:scroll`:

```html
<div x-persist="scrollbar">
    <div class="overflow-y-scroll" wire:scroll>
        <!-- Long scrollable content -->
    </div>
</div>
```

## JavaScript Hooks

Navigation triggers three lifecycle events:

```javascript
document.addEventListener('livewire:navigate', (event) => {
    // Triggered when navigation starts
    // Can prevent navigation: event.preventDefault()
    
    let context = event.detail;
    context.url;     // URL object of destination
    context.history; // True if back/forward navigation
    context.cached;  // True if cached version exists
});

document.addEventListener('livewire:navigating', () => {
    // Triggered before HTML swap
    // Good place to mutate HTML before navigation
});

document.addEventListener('livewire:navigated', () => {
    // Triggered after navigation completes
    // Use instead of DOMContentLoaded
});
```

## Manual Navigation

Trigger navigation programmatically:

```javascript
Livewire.navigate('/new/url');
```

## Event Listeners

Handle persistent event listeners properly:

```javascript
// Run once per navigation
document.addEventListener('livewire:navigated', () => {
    // Page-specific code
}, { once: true });

// Persistent across navigations
document.addEventListener('livewire:navigated', () => {
    // Global code
});
```

## Analytics Integration

Ensure analytics track navigation properly:

```html
<script src="https://cdn.usefathom.com/script.js" 
        data-site="ABCDEFG" 
        data-spa="auto" 
        defer></script>
```

## Script Handling

Control script execution across navigations:

```html
<head>
    <!-- Cache-busted scripts with tracking -->
    <script src="/app.js?id=123" data-navigate-track></script>
</head>

<body>
    <!-- Run only once -->
    <script data-navigate-once>
        console.log('Runs only on first evaluation');
    </script>
</body>
```

## Progress Bar Customization

Configure the loading indicator:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        "showProgressBar": true,
        "progressBarColor": "##2299dd"
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "showProgressBar": true,
        "progressBarColor": "##2299dd"
    }
};
```
{% endtab %}
{% endtabs %}

## Custom Loader Example

Create a custom progress indicator:

```html
<script>
document.addEventListener('livewire:navigate', (event) => {
    if (!window.navigating) {
        window.navigating = true;
        event.preventDefault();
        
        let target = event.detail;
        
        // Show custom loader
        let loader = document.createElement('div');
        loader.className = 'spinner-border text-primary';
        loader.setAttribute('role', 'status');
        loader.innerHTML = '<span class="visually-hidden">Loading...</span>';
        document.getElementById('content').appendChild(loader);
        
        Livewire.navigate(target.url.pathname);
    }
}, { once: true });

document.addEventListener('livewire:navigated', () => {
    window.navigating = false;
});
</script>
```

{% hint style="success" %}
`wire:navigate` often provides 2x faster page loads and creates a JavaScript SPA-like experience while maintaining server-side rendering benefits.
{% endhint %}

{% hint style="warning" %}
Prefetching with `.hover` increases server usage. Use judiciously on high-traffic sites.
{% endhint %}

{% hint style="info" %}
Persisted elements must be placed outside CBWIRE components, typically in your main layout. Use `livewire:navigated` instead of `DOMContentLoaded` for page-specific code.
{% endhint %}