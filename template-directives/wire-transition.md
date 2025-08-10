# wire:transition

CBWIRE offers a smooth way to show or hide elements on your webpage with the **wire:transition** directive. This feature enhances user experience by making elements transition in and out smoothly, rather than just popping into view.

## Example: Showing Blog Comments

For instance, let’s say you’re working on a component that shows comments on a blog post. You want to give users the option to view comments with a nice visual effect. Here’s how you can set it up:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
class extends="cbwire.models.Component" {
    data = {
        "post": {}, // post data loaded here
        "showComments": false
    };

    function toggleComments() {
        data.showComments = !data.showComments;
    }

    function renderIt() {
        return template( "wires.showpost" );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```python
component extends="cbwire.models.Component" {
    data = {
        "post": {}, // post data loaded here
        "showComments": false
    };

    function toggleComments() {
        data.showComments = !data.showComments;
    }

    function renderIt() {
        return template( "wires.showpost" );
    }
}
```
{% endtab %}
{% endtabs %}

In your view, you can use wire:transition to handle the visibility of the comments section:

{% tabs %}
{% tab title="BoxLang" %}
```html
<bx:output>
<div>
    <!-- Other elements... -->

    <button wire:click="toggleComments">Show comments</button>

    <bx:if showComments>
        <div wire:transition>
            <!-- Loop through comments and display them -->
            <bx:loop array="#post.comments#" item="comment">
                <!-- Display comment -->
            </bx:loop>
        </div>
    </bx:if>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<cfoutput>
<div>
    <!-- Other elements... -->

    <button wire:click="toggleComments">Show comments</button>

    <cfif showComments>
        <div wire:transition>
            <!-- Loop through comments and display them -->
            <cfloop array="#post.comments#" item="comment">
                <!-- Display comment -->
            </cfloop>
        </div>
    </cfif>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

With wire:transition, when you click "Show comments," the comments section will gracefully fade into view instead of appearing abruptly.

## Limitations

Currently, wire:transition should be applied to a single conditional element and doesn’t work as expected with a list of dynamic elements, like individual comments in a loop. This is due to the limitations of how transitions are handled in the underlying framework.

## Default Transition Style

By default, elements with wire:transition will fade (change opacity) and slightly scale. Here’s what that looks like:

* **Fading in:** Opacity transitions from 0 to 100%.
* **Scaling in:** Transforms from slightly smaller to full size.

## Customizing Transitions

CBWIRE allows you to customize these transitions to fit your needs. You can specify whether to only apply the transition when elements enter or leave the DOM, adjust the duration and delay of the transitions, or only use certain effects like opacity or scaling:

* **.in**: Only apply when the element appears.
* **.out**: Only apply when the element disappears.
* **.duration.\[?ms]**: Set how long the transition takes in milliseconds.
* **.opacity**: Only use the opacity transition for a simple fade effect without scaling.
* **.scale**: Apply a scaling effect.
* **.origin.\[top|bottom|left|right]**: Set the origin point for the scale effect, useful for dropdowns or popups.

For example, to make a dropdown menu that fades out and scales down from the top when closed, you might set up your transition like this:

```html
<div wire:transition.scale.origin.top.out>
    <!-- Dropdown menu content -->
</div>
```
