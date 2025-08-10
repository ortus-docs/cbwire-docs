# Offline State

### CSS Classes

You can append `.class` to your `wire:offline` annotations and specify a CSS class to toggle when the user is offline.

```javascript
<div wire:offline.class="bg-gold-300"></div>
```

You can also specify classes you want to be removed when offline using `:wire:offline.class.remove`.

```javascript
<div wire:offline.class.remove="bg-gold-300" class="bg-gold-300"></div>
```
