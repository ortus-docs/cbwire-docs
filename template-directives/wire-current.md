# wire:current

The `wire:current` directive allows you to easily detect and style currently active links on a page, making it simple to highlight the current section of your navigation.

## Basic Usage

Add `wire:current` to navigation links to automatically highlight the active link based on the current page URL:

```html
<nav>
    <a href="/dashboard" wire:current="font-bold text-zinc-800">Dashboard</a>
    <a href="/posts" wire:current="font-bold text-zinc-800">Posts</a>
    <a href="/users" wire:current="font-bold text-zinc-800">Users</a>
</nav>
```

When a user visits `/posts`, the "Posts" link will have a stronger font treatment than the other links.

## What wire:current Does

When you add `wire:current` to a link element, CBWIRE automatically:

* **Detects active links**: Compares the link's `href` attribute with the current page URL
* **Applies CSS classes**: Adds the specified classes when the link matches the current page
* **Works with wire:navigate**: Automatically updates active states when using `wire:navigate` for page transitions

## Available Modifiers

### Exact Matching

By default, `wire:current` uses partial matching, meaning it will be applied if the link and current page share the beginning portion of the URL's path. For example, if the link is `/posts`, and the current page is `/posts/1`, the `wire:current` directive will be applied.

Use the `.exact` modifier to require an exact URL match:

```html
<nav>
    <a href="/" wire:current.exact="font-bold">Dashboard</a>
    <a href="/posts" wire:current="font-bold">Posts</a>
</nav>
```

This prevents the "Dashboard" link from being highlighted when the user visits `/posts`.

### Strict Matching

By default, `wire:current` removes trailing slashes (`/`) from its comparison. Use the `.strict` modifier to enforce strict path string comparison:

```html
<nav>
    <a href="/posts/" wire:current.strict="font-bold">Posts</a>
</nav>
```

## Troubleshooting

If `wire:current` is not detecting the current link correctly, ensure the following:

* You have at least one CBWIRE component on the page, or have included `wireScripts()` in your layout
* The link has a valid `href` attribute
* The `href` path matches the URL structure of your application

{% hint style="info" %}
`wire:current` works seamlessly with `wire:navigate` links and automatically updates active states during page transitions.
{% endhint %}
