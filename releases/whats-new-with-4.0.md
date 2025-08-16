# What's New With 4.0

05/16/2024

## New Features

### Livewire.js 3.0 Upgrade

CBWIRE 4.0 features a complete upgrade to Livewire.js version 3, bringing modern frontend capabilities and improved performance. This major upgrade provides the foundation for all new features in this release.

### Alpine.js Integration

Alpine.js is now automatically included with CBWIRE, eliminating the need for separate installation and configuration. This seamless integration provides enhanced frontend interactivity out of the box.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/InteractiveComponent.bx
class extends="cbwire.models.Component" {
    data = {
        "showDetails": false,
        "message": "Click to reveal"
    };
    
    function toggleDetails() {
        data.showDetails = !data.showDetails;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/InteractiveComponent.cfc
component extends="cbwire.models.Component" {
    data = {
        "showDetails" = false,
        "message" = "Click to reveal"
    };
    
    function toggleDetails() {
        data.showDetails = !data.showDetails;
    }
}
```
{% endtab %}
{% endtabs %}

```html
<!-- Template with Alpine.js integration -->
<div x-data="{ expanded: $wire.showDetails }">
    <button wire:click="toggleDetails" @click="expanded = !expanded">
        #message#
    </button>
    <div x-show="expanded" x-transition>
        <p>Additional details revealed!</p>
    </div>
</div>
```

### Lazy Loading Components

Improve application performance with lazy loading capabilities. Components can now be loaded on-demand, reducing initial page load times. While the component loads, a placeholder is displayed to provide visual feedback to users.

```html
<!-- Lazy load a component using wire() with lazy=true -->
#wire("Dashboard", {}, "", true)#

<!-- Lazy load with parameters -->
#wire("UserProfile", { "userId": 123 }, "", true)#

<!-- Lazy load with key and parameters -->
#wire("UserProfile", { "userId": 123 }, "user-profile-key", true)#

<!-- Lazy load with isolated set to false -->
#wire("SharedComponent", {}, "", true, false)#
```

Components can define a `placeholder()` method that returns HTML content to display while the component is loading. This provides a better user experience during the loading process.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/LazyDashboard.bx
class extends="cbwire.models.Component" {
    function placeholder() {
        return "<div>Loading dashboard...</div>";
    }
    
    data = {
        "metrics": [],
        "loaded": false
    };
    
    function onMount() {
        // Simulate loading data
        sleep(1000);
        data.metrics = getMetricsData();
        data.loaded = true;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/LazyDashboard.cfc
component extends="cbwire.models.Component" {
    function placeholder() {
        return "<div>Loading dashboard...</div>";
    }
    
    data = {
        "metrics" = [],
        "loaded" = false
    };
    
    function onMount() {
        // Simulate loading data
        sleep(1000);
        data.metrics = getMetricsData();
        data.loaded = true;
    }
}
```
{% endtab %}
{% endtabs %}

### JavaScript Execution from Actions

Execute JavaScript directly from your component actions using the new `js()` method, enabling seamless server-to-client communication.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/NotificationComponent.bx
class extends="cbwire.models.Component" {
    function showAlert() {
        return js("alert('Action completed successfully!')");
    }
    
    function updateTitle() {
        return js("document.title = 'Updated by CBWIRE'");
    }
    
    function focusInput() {
        return js("document.getElementById('username').focus()");
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/NotificationComponent.cfc
component extends="cbwire.models.Component" {
    function showAlert() {
        return js("alert('Action completed successfully!')");
    }
    
    function updateTitle() {
        return js("document.title = 'Updated by CBWIRE'");
    }
    
    function focusInput() {
        return js("document.getElementById('username').focus()");
    }
}
```
{% endtab %}
{% endtabs %}

### Single-Page Applications with wire:navigate

Transform your application into a single-page application using the new `wire:navigate` directive for seamless navigation without full page reloads.

```html
<!-- Traditional links become SPA navigation -->
<a href="/dashboard" wire:navigate>Dashboard</a>
<a href="/profile" wire:navigate>Profile</a>

<!-- Works with forms and redirects -->
<form wire:submit="updateProfile">
    <input wire:model="name" placeholder="Name">
    <button type="submit">Update Profile</button>
</form>
```

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ProfileComponent.bx
class extends="cbwire.models.Component" {
    function updateProfile() {
        // Save profile logic
        return redirect("/profile", true); // true enables wire:navigate
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ProfileComponent.cfc
component extends="cbwire.models.Component" {
    function updateProfile() {
        // Save profile logic
        return redirect("/profile", true); // true enables wire:navigate
    }
}
```
{% endtab %}
{% endtabs %}

### Content Streaming with wire:stream

Stream content in real-time using the new `wire:stream` directive for dynamic content updates.

```html
<!-- Stream live updates -->
<div wire:stream="notifications">
    <bx:loop array="#notifications#" item="notification">
        <div class="notification">#notification.message#</div>
    </bx:loop>
</div>
```

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/LiveFeed.bx
class extends="cbwire.models.Component" {
    data = {
        "notifications": []
    };
    
    function addNotification(message) {
        data.notifications.append({
            "id": createUUID(),
            "message": message,
            "timestamp": now()
        });
        
        stream("notifications");
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/LiveFeed.cfc
component extends="cbwire.models.Component" {
    data = {
        "notifications" = []
    };
    
    function addNotification(message) {
        arrayAppend(data.notifications, {
            "id" = createUUID(),
            "message" = message,
            "timestamp" = now()
        });
        
        stream("notifications");
    }
}
```
{% endtab %}
{% endtabs %}

### Smooth Transitions with wire:transition

Add smooth animations and transitions to your UI updates using the `wire:transition` directive. This feature enhances user experience by making elements transition in and out smoothly, rather than just popping into view.

```html
<!-- Basic transition (default fade and scale) -->
<div wire:transition>
    <p>This content will fade and scale in/out</p>
</div>

<!-- Opacity-only transition with custom duration -->
<div wire:transition.opacity.duration.300ms>
    <p>This content will fade with 300ms duration</p>
</div>

<!-- Scale transition from top origin -->
<div wire:transition.scale.origin.top.duration.250ms>
    <p>This content will scale from the top</p>
</div>

<!-- Out-only transition -->
<div wire:transition.out.duration.500ms>
    <p>This content will only animate when disappearing</p>
</div>
```

Available modifiers include:
- **Directional**: `.in` (appear only), `.out` (disappear only)
- **Duration**: `.duration.[?ms]` (e.g., `.duration.300ms`)
- **Effects**: `.opacity` (fade only), `.scale` (scale only)
- **Origins**: `.origin.top|bottom|left|right` (scale origin point)

### Request Bundling

Optimize performance with intelligent request bundling that reduces the number of server requests by combining multiple component updates into single requests.

### Complete Engine Rewrite

CBWIRE 4.0 features a completely rewritten engine architecture providing:
- Improved performance and reliability
- Better component lifecycle management
- Enhanced debugging capabilities
- Modern development patterns

## Enhancements

### Lifecycle Method Improvements

Updated lifecycle methods for better consistency and clarity:
- `mount()` renamed to `onMount()`
- `updated()` renamed to `onUpdate()`

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ModernComponent.bx
class extends="cbwire.models.Component" {
    function onMount() {
        // Component initialization logic
        loadInitialData();
    }
    
    function onUpdate() {
        // Called after component updates
        logUpdate();
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ModernComponent.cfc
component extends="cbwire.models.Component" {
    function onMount() {
        // Component initialization logic
        loadInitialData();
    }
    
    function onUpdate() {
        // Called after component updates
        logUpdate();
    }
}
```
{% endtab %}
{% endtabs %}

### Configuration Cleanup

Streamlined configuration by removing deprecated settings:
- Removed `enableTurbo` global setting (superseded by wire:navigate)
- Removed `throwOnMissingSetter` global setting (improved error handling)

### Performance Optimizations

- Faster component rendering and updates
- Optimized request handling with bundling
- Improved memory usage and garbage collection
- Enhanced component lifecycle management

## Breaking Changes

### Lifecycle Methods

If you're upgrading from CBWIRE 3.x, update your lifecycle methods:

```javascript
// Old (3.x)
function mount() { }
function updated() { }

// New (4.x)
function onMount() { }
function onUpdate() { }
```

### Configuration Settings

Remove deprecated settings from your configuration:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// Remove these from config/ColdBox.bx
moduleSettings = {
    "cbwire": {
        // Remove: "enableTurbo": true,
        // Remove: "throwOnMissingSetter": false,
    }
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// Remove these from config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        // Remove: "enableTurbo" = true,
        // Remove: "throwOnMissingSetter" = false,
    }
};
```
{% endtab %}
{% endtabs %}
