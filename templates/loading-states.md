# Loading States

cbwire via Livewire uses AJAX requests to invoke component actions on our wire objects. There are cases when this may involve a long-running process, such as completing a cart checkout, and the page may not respond instantly to a user event like a click. cbwire allows you to effortlessly display loading states, such as showing and hiding elements, adding and removing classes, or toggling HTML attributes until the server responds.

Loading States can make your apps feel responsive and user-friendly.

## Toggling Elements

Let's take a look at a checkout action that takes too long.

```javascript
// File: ./wires/Cart.cfc
component extends="cbwire.models.Component"{

    function checkout(){
        sleep( 5000 ); // optimize this code yo!
    }

    function $renderIt(){
        return this.$renderView( "wires/cart" );
    }
}
```

We can annotate our `<div>` with `wire:loading` to display _Processing Payment_ while the checkout action is running.

```xml
// File: ./views/wires/cart.cfm
<div>
    <button wire:click="checkout">Checkout</button>

    <div wire:loading>
        Processing Payment...
    </div>
</div>
```

After the checkout action completes, the _Processing Payment_ output will disappear. :wave:&#x20;

## Toggling Attributes

```xml
<div>
    <button wire:click="checkout" wire:loading.attr="disabled">
        Checkout
    </button>
</div>
```

## Delay Loading State

Sometimes your component actions complete at the speed of light, and the user doesn't have enough time to see the elements you want to be displayed. You can add a `.delay` modifier to delay showing your HTML elements for _200ms_.

```xml
// File: ./views/wires/cart.cfm
<div wire:loading.delay>...</div>
```

## &#x20;Display Property

Loading State elements are set with a CSS property of `display: inline-block;` by default. You can override this behavior by using various annotation modifiers.

```xml
// File: ./views/wires/cart.cfm
<div wire:loading.flex>...</div>
<div wire:loading.grid>...</div>
<div wire:loading.inline>...</div>
<div wire:loading.table>...</div>
```

## Hide During Loading State

If you would like to display an element EXCEPT during a loading state, you can apply the `wire:loading.remove` annotation.

```xml
// File: ./views/wires/cart.cfm
<div>
    <button wire:click="checkout">Checkout</button>

    <div wire:loading.remove>
        Hide Me While Loading...
    </div>
</div>
```
