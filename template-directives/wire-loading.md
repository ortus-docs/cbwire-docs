# wire:loading

## Overview

Using **wire:loading**, you can show and hide elements in your [templates](../the-essentials/templates.md) while a request is sent to the server.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/MyForm.bx
class extends="cbwire.models.Component" {
    function save() {
        sleep( 2000 ); // pretend we're saving to the database and it's slow
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/MyForm.cfc
component extends="cbwire.models.Component" {
    function save() {
        sleep( 2000 ); // pretend we're saving to the database
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/myform.bxm --->
<div>
    <form wire:submit="save">
        <button type="submit">Save</button>
        <span wire:loading>
            Saving...
        </span>
    </form>
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/myform.cfm --->
<div>
    <form wire:submit="save">
        <button type="submit">Save</button>
        <span wire:loading>
            Saving...
        </span>
    </form>
</div>
```
{% endtab %}
{% endtabs %}

After clicking the save button, you will briefly see the saving loading indicator. Once the save completes, the loading indicator goes away.

## Removing Elements

You can instead show elements by default and hide them during requests to the server using **wire:loading.remove**.

```html
<div>
    <form wire:submit="save">
        <button type="submit" wire:loading.remove>Save</button>
        <span wire:loading>
            Saving...
        </span>
    </form>
</div>
```

Now the save button will disappear until the save completes.

## Toggling Classes

You can toggle classes using **wire:loading.class**.

```html
<div>
    <form wire:submit="save" wire:loading.class="opacity-50">
        <button type="submit">Save</button>
    </form>
</div>
```

You can also remove classes using **wire:loading.class.remove**.

```html
<div>
    <form
        wire:submit="save"
        class="highlighted"
        wire:loading.class.remove="highlighted">
        <button type="submit">Save</button>
    </form>
</div>
```

## Toggling Attributes

You can toggle attributes using **wire:loading.attr**.

```html
<div>
    <button
        type="button"
        wire:click="save"
        wire:loading.attr="disabled">
        Save
    </button>
</div>
```

## Targeting Actions

By default, **wire:loading** will fire when ANY action is called. You can target specific actions using **wire:target**.

```html
<div>
    <form wire:submit="save">
        <button type="button" wire:click="reset">Reset</button>
        <button type="submit">Save</button>
        <span wire:loading wire:target="save">
            Saving...
        </span>
    </form>
</div>
```

The saving loading indicator is displayed when the save button is pressed and not when the reset button is pressed.

You can provide multiple targets to **wire:target**.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/MyForm.bx
class extends="cbwire.models.Component" {
    function save() {
        sleep( 2000 ); // pretend we're saving to the database
    }
    function delete() {
        sleep( 8000 ); // pretend deleting is stupid slow
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/MyForm.cfc
component extends="cbwire.models.Component" {
    function save() {
        sleep( 2000 ); // pretend we're saving to the database
    }
    function delete() {
        sleep( 8000 ); // pretend deleting is stupid slow
    }
}
```
{% endtab %}
{% endtabs %}

```html
<div>
    <form wire:submit="save">
        <button type="button" wire:click="delete">Delete</button>
        <button type="submit">Save</button>
        <span wire:loading wire:target="save,delete">
            Updating...
        </span>
    </form>
</div>
```

## Targeting Parameters

You can specify parameters to match against when using **wire:target="action(param)"**. &#x20;

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/posts.bxm --->
<div>
    <bx:loop array="#posts#" index="post">
        <div wire:key="post-#post.id#">
            <h2>#post.title#</h2>
            <button wire:click="remove( #post.id# )">Remove</button>
            <div wire:loading wire:target="remove( #post.id# )">
                Removing...
            </div>
        </div>
    </bx:loop>
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/posts.cfm --->
<div>
    <cfloop array="#posts#" index="post">
        <div wire:key="post-#post.id#">
            <h2>#post.title#</h2>
            <button wire:click="remove( #post.id# )">Remove</button>
            <div wire:loading wire:target="remove( #post.id# )">
                Removing...
            </div>
        </div>
    </cfloop>
</div>
```
{% endtab %}
{% endtabs %}

Above, our loading indicator is only displayed for the post that is being removed.

{% hint style="danger" %}
Multiple action parameters are currently not supported. This will not work.

```
wire:target="remove(1),add(1)"
```
{% endhint %}

## Targeting Property Updates

You can use **wire:loading** with **wire:model.live** to display elements during data property updates.

```html
<form wire:submit="save">
    <input type="text" wire:model.live="username">
    <div wire:loading wire:target="username">
        Checking username availability...
    </div>
</form>
```

## Excluding Targets

You can exclude targets using **wire:target.except**.

```html
<div>
    <form wire:submit="save">
        <button type="button" wire:click="delete">Delete</button>
        <button type="submit">Save</button>
        <span wire:loading wire:target="save">
            Updating...
        </span>
        <span wire:loading wire:target.except="save">
            Deleting...
        </span>
    </form>

</div>
```

## Customizing Display Values

By default, CBWIRE uses **display: none** to hide elements and **display: inline-block** to show elements.

When toggling elements with a display value other than inline-block, you can use **wire:loading.\[value]**.&#x20;

Below are the available display values:

```html
<div wire:loading.inline-flex>...</div>
<div wire:loading.inline>...</div>
<div wire:loading.block>...</div>
<div wire:loading.table>...</div>
<div wire:loading.flex>...</div>
<div wire:loading.grid>...</div>
```

## Delaying Loading Indicators

Sometimes, loading indicators are so fast that they only display on the screen briefly before being removed. This can be jarring and distracting to users.

You can delay showing an indicator using **wire:loading.delay**.

```html
<div wire:loading.delay>...</div>
```

**This will prevent the indicator from showing unless the server request takes 200ms or more.**

There are built-in interval aliases you can use as well.

```html
<div wire:loading.delay.shortest>...</div> <!-- 50ms -->
<div wire:loading.delay.shorter>...</div>  <!-- 100ms -->
<div wire:loading.delay.short>...</div>    <!-- 150ms -->
<div wire:loading.delay>...</div>          <!-- 200ms -->
<div wire:loading.delay.long>...</div>     <!-- 300ms -->
<div wire:loading.delay.longer>...</div>   <!-- 500ms -->
<div wire:loading.delay.longest>...</div>  <!-- 1000ms -->
```
