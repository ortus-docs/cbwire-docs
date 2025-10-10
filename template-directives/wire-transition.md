# wire:transition

CBWIRE offers a smooth way to show or hide elements on your webpage with the `wire:transition` directive. This feature enhances user experience by making elements transition in and out smoothly, rather than just popping into view.

## Basic Usage

This interactive dashboard component demonstrates multiple `wire:transition` features including basic transitions, directional control, duration customization, and different effect types:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Dashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "showStats": false,
        "showNotifications": false,
        "showModal": false
    };

    function toggleStats() {
        data.showStats = !data.showStats;
    }

    function toggleNotifications() {
        data.showNotifications = !data.showNotifications;
    }

    function showModal() {
        data.showModal = true;
    }

    function closeModal() {
        data.showModal = false;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Dashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "showStats" = false,
        "showNotifications" = false,
        "showModal" = false
    };

    function toggleStats() {
        data.showStats = !data.showStats;
    }

    function toggleNotifications() {
        data.showNotifications = !data.showNotifications;
    }

    function showModal() {
        data.showModal = true;
    }

    function closeModal() {
        data.showModal = false;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/dashboard.bxm -->
<bx:output>
<div class="dashboard">
    <h1>Dashboard</h1>
    
    <div class="controls">
        <button wire:click="toggleStats">
            <bx:if showStats>Hide<bx:else>Show</bx:if> Stats
        </button>
        <button wire:click="toggleNotifications">Notifications</button>
        <button wire:click="showModal">Open Modal</button>
    </div>

    <!-- Basic transition with default fade and scale -->
    <bx:if showStats>
        <div wire:transition class="stats-panel">
            <h3>Statistics</h3>
            <p>Users: 1,234 | Sales: $56,789</p>
        </div>
    </bx:if>

    <!-- Opacity-only transition with custom duration -->
    <bx:if showNotifications>
        <div wire:transition.opacity.duration.300ms class="notifications">
            <h3>Recent Notifications</h3>
            <div class="notification">New user registered</div>
            <div class="notification">Payment received</div>
        </div>
    </bx:if>

    <!-- Modal with scale transition from top -->
    <bx:if showModal>
        <div class="modal-backdrop">
            <div wire:transition.scale.origin.top.duration.250ms class="modal">
                <h3>Confirm Action</h3>
                <p>Are you sure you want to proceed?</p>
                <button wire:click="closeModal">Cancel</button>
                <button wire:click="closeModal">Confirm</button>
            </div>
        </div>
    </bx:if>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/dashboard.cfm -->
<cfoutput>
<div class="dashboard">
    <h1>Dashboard</h1>
    
    <div class="controls">
        <button wire:click="toggleStats">
            <cfif showStats>Hide<cfelse>Show</cfif> Stats
        </button>
        <button wire:click="toggleNotifications">Notifications</button>
        <button wire:click="showModal">Open Modal</button>
    </div>

    <!-- Basic transition with default fade and scale -->
    <cfif showStats>
        <div wire:transition class="stats-panel">
            <h3>Statistics</h3>
            <p>Users: 1,234 | Sales: $56,789</p>
        </div>
    </cfif>

    <!-- Opacity-only transition with custom duration -->
    <cfif showNotifications>
        <div wire:transition.opacity.duration.300ms class="notifications">
            <h3>Recent Notifications</h3>
            <div class="notification">New user registered</div>
            <div class="notification">Payment received</div>
        </div>
    </cfif>

    <!-- Modal with scale transition from top -->
    <cfif showModal>
        <div class="modal-backdrop">
            <div wire:transition.scale.origin.top.duration.250ms class="modal">
                <h3>Confirm Action</h3>
                <p>Are you sure you want to proceed?</p>
                <button wire:click="closeModal">Cancel</button>
                <button wire:click="closeModal">Confirm</button>
            </div>
        </div>
    </cfif>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:transition Does

When you add `wire:transition` to an element, CBWIRE automatically:

* **Fades in/out**: Changes opacity from 0 to 100% (and vice versa)
* **Scales**: Transforms from slightly smaller to full size by default
* **Smooths interactions**: Makes show/hide operations feel polished instead of abrupt

## Available Modifiers

You can customize transitions with these modifiers:

* **Directional**: `.in` (appear only), `.out` (disappear only)
* **Duration**: `.duration.[?ms]` (e.g., `.duration.300ms`)
* **Effects**: `.opacity` (fade only), `.scale` (scale only)
* **Origins**: `.origin.top|bottom|left|right` (scale origin point)

Combine modifiers as needed: `wire:transition.opacity.out.duration.300ms`

## Troubleshooting

### Transitions Not Working

If you're not seeing transition effects, it may be because Livewire is having trouble with DOM diffing. You can often fix this by adding `wire:key` to the same element that has `wire:transition`:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- If this doesn't transition properly -->
<bx:if showPanel>
    <div wire:transition>
        Panel content
    </div>
</bx:if>

<!-- Try adding wire:key -->
<bx:if showPanel>
    <div wire:transition wire:key="panel-content">
        Panel content
    </div>
</bx:if>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- If this doesn't transition properly -->
<cfif showPanel>
    <div wire:transition>
        Panel content
    </div>
</cfif>

<!-- Try adding wire:key -->
<cfif showPanel>
    <div wire:transition wire:key="panel-content">
        Panel content
    </div>
</cfif>
```
{% endtab %}
{% endtabs %}

This helps Livewire properly track the element across updates and apply transitions correctly.

## Limitations

Currently, `wire:transition` should be applied to a single conditional element and doesn't work as expected with a list of dynamic elements, like individual comments in a loop. This is due to the limitations of how transitions are handled in the underlying framework.

{% tabs %}
{% tab title="BoxLang" %}
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
{% endtab %}

{% tab title="CFML" %}
```html
<!-- ✅ Works well -->
<cfif showPanel>
    <div wire:transition>Panel content</div>
</cfif>

<!-- ❌ Not recommended -->
<cfloop array="#items#" index="item">
    <div wire:transition>Item content</div>
</cfloop>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Use `wire:transition` for show/hide scenarios on single elements rather than complex list animations for the best results.
{% endhint %}
