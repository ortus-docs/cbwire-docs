# wire:transition

CBWIRE offers a smooth way to show or hide elements on your webpage with the `wire:transition` directive. This feature enhances user experience by making elements transition in and out smoothly, rather than just popping into view.

## Basic Usage

Use `wire:transition` on elements that appear or disappear based on your component's state. When you toggle the visibility of an element, it will gracefully fade into view instead of appearing abruptly.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/BlogPost.bx
class extends="cbwire.models.Component" {
    data = {
        "post": {},
        "showComments": false
    };

    function toggleComments() {
        data.showComments = !data.showComments;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/BlogPost.cfc
component extends="cbwire.models.Component" {
    data = {
        "post" = {},
        "showComments" = false
    };

    function toggleComments() {
        data.showComments = !data.showComments;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/blogPost.bxm -->
<bx:output>
<div>
    <h1>#data.post.title#</h1>
    <p>#data.post.content#</p>

    <button wire:click="toggleComments">Show Comments</button>

    <bx:if data.showComments>
        <div wire:transition>
            <h3>Comments</h3>
            <bx:loop array="#data.post.comments#" index="comment">
                <div class="comment">
                    <strong>#comment.author#:</strong>
                    <p>#comment.text#</p>
                </div>
            </bx:loop>
        </div>
    </bx:if>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/blogPost.cfm -->
<cfoutput>
<div>
    <h1>#data.post.title#</h1>
    <p>#data.post.content#</p>

    <button wire:click="toggleComments">Show Comments</button>

    <cfif data.showComments>
        <div wire:transition>
            <h3>Comments</h3>
            <cfloop array="#data.post.comments#" index="comment">
                <div class="comment">
                    <strong>#comment.author#:</strong>
                    <p>#comment.text#</p>
                </div>
            </cfloop>
        </div>
    </cfif>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Default Transition Style

By default, elements with `wire:transition` will fade (change opacity) and slightly scale:

* **Fading in:** Opacity transitions from 0 to 100%
* **Scaling in:** Transforms from slightly smaller to full size

## Transition Modifiers

CBWIRE allows you to customize these transitions to fit your needs:

### Directional Control

* **`.in`**: Only apply when the element appears
* **`.out`**: Only apply when the element disappears

```html
<!-- Only animate when appearing -->
<div wire:transition.in>
    Content appears with transition, disappears instantly
</div>

<!-- Only animate when disappearing -->
<div wire:transition.out>
    Content appears instantly, disappears with transition  
</div>
```

### Duration Control

* **`.duration.[?ms]`**: Set how long the transition takes in milliseconds

```html
<!-- Quick transition -->
<div wire:transition.duration.100ms>
    Fast appearing content
</div>

<!-- Slower transition -->
<div wire:transition.duration.500ms>
    Slowly appearing content
</div>
```

### Effect Types

* **`.opacity`**: Only use the opacity transition for a simple fade effect without scaling
* **`.scale`**: Apply a scaling effect

```html
<!-- Fade only -->
<div wire:transition.opacity>
    Simple fade in/out
</div>

<!-- Scale only -->
<div wire:transition.scale>
    Scaling effect
</div>
```

### Scale Origins

* **`.origin.[top|bottom|left|right]`**: Set the origin point for the scale effect, useful for dropdowns or popups

```html
<!-- Scale from top - perfect for dropdowns -->
<div wire:transition.scale.origin.top>
    Dropdown menu content
</div>

<!-- Scale from bottom -->
<div wire:transition.scale.origin.bottom>
    Bottom popup content
</div>
```

## Combining Modifiers

You can combine multiple modifiers to create the exact transition you need:

```html
<!-- Fade out only, taking 300ms -->
<div wire:transition.opacity.out.duration.300ms>
    Fades out slowly when hidden
</div>

<!-- Scale from top when appearing, quick duration -->
<div wire:transition.scale.origin.top.in.duration.150ms>
    Quick dropdown entrance
</div>
```

## Practical Examples

### Modal Dialog

```html
<bx:if data.showModal>
    <div class="modal-backdrop">
        <div wire:transition.scale.origin.top class="modal-content">
            <h2>Confirm Action</h2>
            <p>Are you sure you want to proceed?</p>
            <button wire:click="closeModal">Cancel</button>
            <button wire:click="confirmAction">Confirm</button>
        </div>
    </div>
</bx:if>
```

### Notification Panel

```html
<bx:if data.showNotifications>
    <div wire:transition.opacity.duration.200ms class="notification-panel">
        <h3>Recent Notifications</h3>
        <bx:loop array="#data.notifications#" index="notification">
            <div class="notification-item">
                #notification.message#
            </div>
        </bx:loop>
    </div>
</bx:if>
```

### Expandable Content

```html
<button wire:click="toggleDetails">
    <bx:if data.showDetails>Hide<bx:else>Show</bx:if> Details
</button>

<bx:if data.showDetails>
    <div wire:transition.scale.origin.top.duration.250ms>
        <p>Additional content that expands from the top...</p>
    </div>
</bx:if>
```

## Limitations

Currently, `wire:transition` should be applied to a single conditional element and doesn't work as expected with a list of dynamic elements, like individual comments in a loop. This is due to the limitations of how transitions are handled in the underlying framework.

```html
<!-- ✅ Works well -->
<bx:if showPanel>
    <div wire:transition>Panel content</div>
</bx:if>

<!-- ❌ Not recommended -->
<bx:loop array="#items#" index="item">
    <div wire:transition>Item content</div>
</bx:loop>
```

{% hint style="info" %}
Use `wire:transition` for show/hide scenarios on single elements rather than complex list animations for the best results.
{% endhint %}