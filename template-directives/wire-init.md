# wire:init

The `wire:init` directive runs an action automatically when the component is first rendered in the browser. This allows you to load data or perform initialization tasks after the page loads without blocking the initial page render, creating a better user experience for data-heavy components.

## Basic Usage

This analytics dashboard demonstrates `wire:init` loading data after the component renders:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/AnalyticsDashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "stats": {},
        "loading": true
    };

    function loadStats() {
        sleep(1500); // Simulate API call
        data.stats = {
            "users": 1234,
            "sales": 56789,
            "revenue": 98765
        };
        data.loading = false;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/AnalyticsDashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "stats" = {},
        "loading" = true
    };

    function loadStats() {
        sleep(1500); // Simulate API call
        data.stats = {
            "users" = 1234,
            "sales" = 56789,
            "revenue" = 98765
        };
        data.loading = false;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/analyticsDashboard.bxm -->
<bx:output>
<div wire:init="loadStats">
    <h1>Analytics Dashboard</h1>
    
    <bx:if loading>
        <div>Loading analytics data...</div>
    <bx:else>
        <div class="stats">
            <div>Users: #stats.users#</div>
            <div>Sales: #stats.sales#</div>
            <div>Revenue: $#stats.revenue#</div>
        </div>
    </bx:if>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/analyticsDashboard.cfm -->
<cfoutput>
<div wire:init="loadStats">
    <h1>Analytics Dashboard</h1>
    
    <cfif loading>
        <div>Loading analytics data...</div>
    <cfelse>
        <div class="stats">
            <div>Users: #stats.users#</div>
            <div>Sales: #stats.sales#</div>
            <div>Revenue: $#stats.revenue#</div>
        </div>
    </cfif>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:init Does

When you add `wire:init` to an element, CBWIRE automatically:

- **Runs After Render**: Executes the specified action immediately after the component renders
- **Enables Lazy Loading**: Allows heavy data operations without blocking initial page load
- **Triggers Once**: Runs the action only on the initial render, not on subsequent updates
- **Improves Performance**: Separates fast rendering from slow data loading

## Available Modifiers

The `wire:init` directive does not support any modifiers.

{% hint style="info" %}
Consider using [Lazy Loading](../features/lazy-loading.md) as a modern alternative that provides more control over when and how components initialize.
{% endhint %}