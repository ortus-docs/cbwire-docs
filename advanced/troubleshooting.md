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

## Server Configuration Issues

### 500 Error Loading livewire.js with New ColdBox Template Layout

**Problem**: CBWIRE returns a 500 error when trying to load `livewire.js` after switching to the newer ColdBox template layout.

**Root Cause**: The newer ColdBox template layout installs modules under `./lib/modules/` instead of the traditional `./modules/` directory. CBWIRE references `/modules/cbwire/includes/js/livewire.js`, which no longer maps to a valid path, causing the 500 error.

**Solution**: Add a `/modules` alias in your `server.json` so that CommandBox maps requests for `/modules` to the new `./lib/modules/` directory:

```json
{
    "web": {
        "aliases": {
            "/modules": "./lib/modules/"
        }
    }
}
```

Restart your CommandBox server after making this change:

```bash
box restart
```

{% hint style="info" %}
This alias approach applies to **CommandBox-managed servers**. If you are running a different web server (e.g., IIS, Apache, nginx), you will need to create an equivalent URL alias or virtual directory using your web server's configuration.
{% endhint %}

{% hint style="info" %}
If you prefer to use the traditional `./modules/` layout, you can scaffold your project from the [flat ColdBox template](https://github.com/coldbox-templates/flat), which keeps modules in the conventional location and does not require this alias.
{% endhint %}

### 400 Bad Request on CBWIRE Updates

**Problem**: The `/cbwire/update` endpoint returns "400 Bad Request" errors, preventing CBWIRE components from updating.

**Root Cause**: Server software or connectors stripping out the `X-Livewire` header that CBWIRE requires for request handling.

**Error**: HTTP 400 status code when CBWIRE attempts to process component updates.

**Solution**: Configure your server software to allow empty headers or preserve the `X-Livewire` header.

#### BonCode AJP Connector (IIS + Lucee)

If you're running Lucee behind IIS using the BonCode AJP Connector, you need to allow empty headers:

**File**: `/BIN/BonCodeAJP13.settings`

```xml
<!-- Change from False to True -->
<AllowEmptyHeaders>True</AllowEmptyHeaders>
```

After making this change, restart your web server for the setting to take effect.

#### Other Server Software

The `X-Livewire` header is essential for CBWIRE to function properly. If you're experiencing 400 errors with other server configurations:

1. **Check server logs** - Look for messages about rejected or stripped headers
2. **Review proxy settings** - Ensure reverse proxies (nginx, Apache) pass through the `X-Livewire` header
3. **Check security modules** - WAF or security modules might be filtering custom headers
4. **Test header presence** - Use browser developer tools to verify the `X-Livewire` header is being sent

**Example nginx configuration** to preserve headers:

```nginx
location /cbwire/ {
    proxy_pass http://your-backend;
    proxy_set_header X-Livewire $http_x_livewire;
    proxy_pass_request_headers on;
}
```

**Example Apache configuration** to preserve headers:

```apache
<Location /cbwire/>
    ProxyPreserveHost On
    ProxyPass http://your-backend/cbwire/
    ProxyPassReverse http://your-backend/cbwire/
</Location>
```

{% hint style="warning" %}
CBWIRE depends on the `X-Livewire` header for proper request routing and component identification. If this header is stripped or blocked by your server configuration, CBWIRE will return 400 errors and components will fail to update.
{% endhint %}

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

| Directive | Behavior                       | Use with CBWIRE                |
| --------- | ------------------------------ | ------------------------------ |
| `x-if`    | Removes/recreates DOM elements | ❌ Breaks component tracking    |
| `x-show`  | Toggles CSS `display` property | ✅ Preserves component tracking |

## Quick Reference

### Essential Rules

1. **Placeholder-Template Matching**: Outer elements must be identical
2. **Conditional Blocks**: Wrap complex conditionals with `<!--[if BLOCK]><![endif]-->` markers
3. **Alpine Quotes**: Always use single quotes inside `x-data` attributes
4. **Component Visibility**: Use `x-show` instead of `x-if` with CBWIRE components

### Common Error Messages

| Error                      | Likely Cause                              | Solution                                    |
| -------------------------- | ----------------------------------------- | ------------------------------------------- |
| "Snapshot missing"         | Placeholder/template mismatch             | Match outer elements exactly                |
| "Unable to find component" | Alpine `x-if` removing components         | Use `x-show` instead                        |
| JavaScript syntax error    | Double quotes in `x-data`                 | Use single quotes                           |
| 400 Bad Request            | `X-Livewire` header stripped by server    | Configure server to allow empty headers     |
| 500 Error (livewire.js)    | New ColdBox layout moves modules to `lib/modules/` | Add `/modules` alias in `server.json` |

{% hint style="info" %}
When in doubt, add `wire:key` to dynamic elements and wrap conditionals with block markers. These techniques help Livewire's DOM diffing engine track changes accurately.
{% endhint %}
