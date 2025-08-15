# Troubleshooting

CBWIRE integrates complex technologies like Livewire's DOM diffing, Alpine.js reactivity, and server-side rendering. This guide addresses the most common issues you'll encounter and provides practical solutions with detailed examples.

## CBWIRE Component Issues

### Lazy-Loaded Components with Placeholders Don't Render

**Problem**: You get a "Snapshot missing on Livewire component" error when using lazy loading with placeholders.

**Root Cause**: Mismatch between placeholder and template outer elements confuses Livewire's DOM diffing engine.

**Error**: `Snapshot missing on Livewire component with id...`

**Solution**: Match outer elements exactly between placeholder and template.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Widget.bx
class extends="cbwire.models.Component" {
    function placeholder() {
        return "<div>put spinner here...</div>"; // ✅ Matches template
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Widget.cfc
component extends="cbwire.models.Component" {
    function placeholder() {
        return "<div>put spinner here...</div>"; // ✅ Matches template
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/widget.bxm -->
<bx:output>
<div> <!-- ✅ Matches placeholder's <div> -->
    <h1>My Widget</h1>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/widget.cfm -->
<cfoutput>
<div> <!-- ✅ Matches placeholder's <div> -->
    <h1>My Widget</h1>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

### Conditional Rendering Issues

**Problem**: Livewire fails to detect conditional sections appearing/disappearing, causing rendering glitches.

**Root Cause**: Complex nested conditionals can confuse Livewire's DOM diffing algorithm.

**Solution**: Add `wire:key` to conditional elements (recommended approach).

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userForm.bxm -->
<bx:output>
<div>
    <h1>User Registration</h1>
    
    <!-- ✅ Using wire:key (recommended) -->
    <bx:if errorList.len()>
        <div class="alert alert-danger" role="alert" wire:key="error-alert">
            <p>Please fix the following errors:</p>
            <ul>
                <bx:loop array="#errorList#" index="error">
                    <li>#error#</li>
                </bx:loop>
            </ul>
        </div>
    </bx:if>
    
    <form wire:submit="submit">
        <input type="text" wire:model="name" placeholder="Name">
        <input type="email" wire:model="email" placeholder="Email">
        <button type="submit">Register</button>
    </form>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userForm.cfm -->
<cfoutput>
<div>
    <h1>User Registration</h1>
    
    <!-- ✅ Using wire:key (recommended) -->
    <cfif arrayLen(errorList)>
        <div class="alert alert-danger" role="alert" wire:key="error-alert">
            <p>Please fix the following errors:</p>
            <ul>
                <cfloop array="#errorList#" index="error">
                    <li>#error#</li>
                </cfloop>
            </ul>
        </div>
    </cfif>
    
    <form wire:submit="submit">
        <input type="text" wire:model="name" placeholder="Name">
        <input type="email" wire:model="email" placeholder="Email">
        <button type="submit">Register</button>
    </form>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

**Alternative Solution**: If `wire:key` doesn't solve the issue, add conditional block markers.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userForm.bxm -->
<bx:output>
<div>
    <h1>User Registration</h1>
    
    <!-- ✅ Wrapped conditional with block markers -->
    <!--[if BLOCK]><![endif]-->
    <bx:if errorList.len()>
        <div class="alert alert-danger" role="alert">
            <p>Please fix the following errors:</p>
            <ul>
                <bx:loop array="#errorList#" index="error">
                    <li>#error#</li>
                </bx:loop>
            </ul>
        </div>
    </bx:if>
    <!--[if ENDBLOCK]><![endif]-->
    
    <form wire:submit="submit">
        <input type="text" wire:model="name" placeholder="Name">
        <input type="email" wire:model="email" placeholder="Email">
        <button type="submit">Register</button>
    </form>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userForm.cfm -->
<cfoutput>
<div>
    <h1>User Registration</h1>
    
    <!-- ✅ Wrapped conditional with block markers -->
    <!--[if BLOCK]><![endif]-->
    <cfif arrayLen(errorList)>
        <div class="alert alert-danger" role="alert">
            <p>Please fix the following errors:</p>
            <ul>
                <cfloop array="#errorList#" index="error">
                    <li>#error#</li>
                </cfloop>
            </ul>
        </div>
    </cfif>
    <!--[if ENDBLOCK]><![endif]-->
    
    <form wire:submit="submit">
        <input type="text" wire:model="name" placeholder="Name">
        <input type="email" wire:model="email" placeholder="Email">
        <button type="submit">Register</button>
    </form>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

**Block Marker Syntax**:
```html
<!--[if BLOCK]><![endif]-->
<!-- Your conditional content here -->
<!--[if ENDBLOCK]><![endif]-->
```

## Alpine.js Integration Issues

### Quote Syntax Errors in x-data

**Problem**: JavaScript errors when using double quotes inside `x-data` attributes.

**Root Cause**: HTML attribute already uses double quotes, creating syntax conflicts.

**Error**: JavaScript syntax error in browser console.

**Solution**: Use single quotes inside `x-data` blocks.

```html
<!-- ✅ Correct syntax -->
<div x-data="{
    name: 'CBWIRE',           // ✅ Single quotes work properly
    message: 'Welcome user',  // ✅ Single quotes work properly
    count: 0,
    increment() { this.count++ }
}">
    <div>Name: <span x-text="name"></span></div>
    <div>Message: <span x-text="message"></span></div>
    <div>Count: <span x-text="count"></span></div>
    <button @click="increment">Increment</button>
</div>
```

### Component Removal with Alpine x-if

**Problem**: "Unable to find component" errors when using `x-if` with CBWIRE components.

**Root Cause**: Alpine's `x-if` completely removes/recreates DOM elements, breaking Livewire's component tracking.

**Error**: `Unable to find component K8SDFLSDF902KSDFLASKJFASDFLJ.`

**Solution**: Replace `x-if` with `x-show` to preserve DOM elements.

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/dashboard.bxm -->
<bx:output>
<div x-data="{
    loading: false,
    async init() {
        this.loading = true;
        await $wire.loadData();
        this.loading = false;
    }
}">
    <h1>Dashboard</h1>
    
    <!-- ✅ x-show preserves DOM elements -->
    <div x-show="loading">
        #wire("Spinner")#  <!-- ✅ Component stays tracked -->
    </div>
    
    <div x-show="!loading">
        <p>Dashboard content loaded!</p>
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/dashboard.cfm -->
<cfoutput>
<div x-data="{
    loading: false,
    async init() {
        this.loading = true;
        await $wire.loadData();
        this.loading = false;
    }
}">
    <h1>Dashboard</h1>
    
    <!-- ✅ x-show preserves DOM elements -->
    <div x-show="loading">
        #wire("Spinner")#  <!-- ✅ Component stays tracked -->
    </div>
    
    <div x-show="!loading">
        <p>Dashboard content loaded!</p>
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Key Differences: x-if vs x-show

| Directive | Behavior | Use with CBWIRE |
|-----------|----------|-----------------|
| `x-if` | Removes/recreates DOM elements | ❌ Breaks component tracking |
| `x-show` | Toggles CSS `display` property | ✅ Preserves component tracking |

## Quick Reference

### Essential Rules

1. **Placeholder-Template Matching**: Outer elements must be identical
2. **Conditional Blocks**: Wrap complex conditionals with `<!--[if BLOCK]><![endif]-->` markers
3. **Alpine Quotes**: Always use single quotes inside `x-data` attributes
4. **Component Visibility**: Use `x-show` instead of `x-if` with CBWIRE components

### Common Error Messages

| Error | Likely Cause | Solution |
|-------|--------------|----------|
| "Snapshot missing" | Placeholder/template mismatch | Match outer elements exactly |
| "Unable to find component" | Alpine `x-if` removing components | Use `x-show` instead |
| JavaScript syntax error | Double quotes in `x-data` | Use single quotes |

{% hint style="info" %}
When in doubt, add `wire:key` to dynamic elements and wrap conditionals with block markers. These techniques help Livewire's DOM diffing engine track changes accurately.
{% endhint %}