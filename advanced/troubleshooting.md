# Troubleshooting

CBWIRE integrates complex technologies like Livewire's DOM diffing, Alpine.js reactivity, and server-side rendering. This guide addresses the most common issues you'll encounter and provides practical solutions with detailed examples.

## CBWIRE Component Issues

### Lazy-Loaded Components with Placeholders Don't Render

**Problem**: You get a "Snapshot missing on Livewire component" error when using lazy loading with placeholders.

**Root Cause**: Mismatch between placeholder and template outer elements confuses Livewire's DOM diffing engine.

**Example Problem**:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Widget.bx
class extends="cbwire.models.Component" {
    function placeholder() {
        return "<section>put spinner here...</section>"; // ❌ Wrong outer element
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Widget.cfc
component extends="cbwire.models.Component" {
    function placeholder() {
        return "<section>put spinner here...</section>"; // ❌ Wrong outer element
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
<div> <!-- ❌ Different from placeholder's <section> -->
    <h1>My Widget</h1>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/widget.cfm -->
<cfoutput>
<div> <!-- ❌ Different from placeholder's <section> -->
    <h1>My Widget</h1>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

**Usage**:
```html
#wire(name="Widget", lazy=true)#
```

**Error**: 
```
Snapshot missing on Livewire component with id K8SDFLSDF902KSDFLASKJFASDFLJ.
```

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

**Example Problem**:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserForm.bx
class extends="cbwire.models.Component" {
    data = {
        "errorList": [],
        "name": "",
        "email": ""
    };

    function submit() {
        data.errorList = [];
        
        if (!data.name.len()) {
            data.errorList.append("Name is required");
        }
        
        if (!data.email.len()) {
            data.errorList.append("Email is required");
        }
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "errorList" = [],
        "name" = "",
        "email" = ""
    };

    function submit() {
        data.errorList = [];
        
        if (!len(data.name)) {
            arrayAppend(data.errorList, "Name is required");
        }
        
        if (!len(data.email)) {
            arrayAppend(data.errorList, "Email is required");
        }
    }
}
```
{% endtab %}
{% endtabs %}

**Problematic Template**:

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userForm.bxm -->
<bx:output>
<div>
    <h1>User Registration</h1>
    
    <!-- ❌ Conditional block without markers -->
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
    
    <!-- ❌ Conditional block without markers -->
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

**Solution**: Add conditional block markers to help Livewire track dynamic sections.

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

**Example Problem**:

```html
<!-- ❌ Will cause JavaScript error -->
<div x-data="{
    name: "CBWIRE",           // ❌ Double quotes conflict
    message: "Welcome user"   // ❌ Double quotes conflict
}">
    <div>Name: <span x-text="name"></span></div>
    <div>Message: <span x-text="message"></span></div>
</div>
```

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

**Example Problem**:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/Dashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "loading": false
    };

    function loadData() {
        data.loading = true;
        sleep(2000); // Simulate slow operation
        data.loading = false;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/Dashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "loading" = false
    };

    function loadData() {
        data.loading = true;
        sleep(2000); // Simulate slow operation
        data.loading = false;
    }
}
```
{% endtab %}
{% endtabs %}

**Problematic Template**:

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
    
    <!-- ❌ x-if completely removes/recreates DOM -->
    <template x-if="loading">
        #wire("Spinner")#  <!-- ❌ Component gets lost during recreation -->
    </template>
    
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
    
    <!-- ❌ x-if completely removes/recreates DOM -->
    <template x-if="loading">
        #wire("Spinner")#  <!-- ❌ Component gets lost during recreation -->
    </template>
    
    <div x-show="!loading">
        <p>Dashboard content loaded!</p>
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

**Error**: 
```
Unable to find component K8SDFLSDF902KSDFLASKJFASDFLJ.
```

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