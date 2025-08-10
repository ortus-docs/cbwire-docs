# wire:offline

CBWIRE provides offline state management, making it easy to toggle UI elements when the user is offline or online.

You can use **wire:offline** to display elements when CBWIRE detects that the user is offline.

Copy

```html
<div wire:offline><!-- Oh no, you're offline --></div>
```

You can append _.class_ to your wire:offline directive and specify a CSS class to toggle when the user is offline.

Copy

```html
<div wire:offline.class="online" class="online"></div>
```

Use _.remove_ to specify classes you want to be removed when offline.

Copy

```html
<div wire:offline.class.remove="im-online" class="im-online"></div>
```
