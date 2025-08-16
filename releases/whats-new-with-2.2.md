# What's New With 2.2

01/09/2022

## New Features

### Hydration Lifecycle Hooks

CBWIRE 2.2 introduces new hydration lifecycle methods that run when components are restored from their serialized state.

```javascript
// wires/UserForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "user" = {},
        "originalEmail" = ""
    };
    
    function onMount(params = {}) {
        data.user = getUser(params.userId);
        data.originalEmail = data.user.email;
    }
    
    function onHydrate() {
        // Called every time component is hydrated from state
        logActivity("Form hydrated for user: " & data.user.name);
    }
    
    function onHydrateEmail(value, oldValue) {
        // Called when 'email' property is hydrated
        if (value != data.originalEmail) {
            data.emailChanged = true;
        }
    }
}
```

### Auto-Trim Data Properties

All string data properties are now automatically trimmed of whitespace, ensuring cleaner data handling.

```javascript
// wires/ContactForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "",
        "email" = "",
        "message" = ""
    };
    
    function submitForm() {
        // All properties are automatically trimmed
        // "  John Doe  " becomes "John Doe"
        // "  test@email.com  " becomes "test@email.com"
        
        if (len(data.name) && len(data.email)) {
            sendContactMessage(data);
        }
    }
}
```

### JavaScript Component Access

Components can now be accessed directly from JavaScript using the global `cbwire.find()` method.

```html
<!-- wires/dashboard.cfm -->
<cfoutput>
<div id="dashboard-#args._id#">
    <h2>Dashboard</h2>
    <p>Counter: #counter#</p>
    <button wire:click="increment">Increment</button>
    <button onclick="resetFromJS('#args._id#')">Reset via JS</button>
</div>
</cfoutput>

<script>
function resetFromJS(componentId) {
    // Access component directly from JavaScript
    var component = cbwire.find(componentId);
    component.call('reset');
}

// You can also access component data
var dashboard = cbwire.find('#args._id#');
console.log('Current counter:', dashboard.data.counter);
</script>
```

### Turbo SPA Support

New `enableTurbo` setting provides single-page application functionality for faster navigation.

```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "enableTurbo" = true  // Enable SPA-like navigation
    }
};
```

When enabled, page navigation becomes faster with JavaScript-powered transitions instead of full page reloads.

### Enhanced Reset Functionality

The `reset()` method can now reset all data properties when called without parameters.

```javascript
// wires/MultiStepForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "step" = 1,
        "firstName" = "",
        "lastName" = "",
        "email" = "",
        "phone" = ""
    };
    
    function resetForm() {
        // Reset all data properties to their initial values
        reset();
    }
    
    function resetContactInfo() {
        // Reset only specific properties
        reset(["email", "phone"]);
    }
    
    function nextStep() {
        data.step += 1;
    }
    
    function previousStep() {
        data.step -= 1;
    }
}
```

## Enhancements

### Lifecycle Method Standardization

Updated lifecycle method naming for consistency - `mount()` is now `onMount()`.

```javascript
// Old syntax (2.1 and earlier)
component extends="cbwire.models.Component" {
    function mount(params = {}) {
        // Component initialization
    }
}

// New syntax (2.2+)
component extends="cbwire.models.Component" {
    function onMount(params = {}) {
        // Component initialization
    }
}
```

### Improved Performance

- Faster hydration process with optimized component restoration
- Better memory management for long-running applications
- Streamlined event handling and data binding

## Bug Fixes

### Event System Improvements

- **Fixed immediate listener firing**: Event listeners no longer fire immediately when emitting from the same component
- **Proper event ordering**: Ensured `onHydrate()` runs before component actions
- **Computed property timing**: Fixed computed properties not rendering correctly before actions execute

### Documentation and Structure

- **Fixed DocBox integration**: Resolved documentation generation issues caused by file structure changes
- **Better error handling**: Improved error messages and debugging information

### Component Lifecycle

- **Hydration timing**: Ensured proper order of hydration lifecycle methods
- **State restoration**: Fixed issues with component state not being properly restored in certain scenarios

## Breaking Changes

### Lifecycle Method Names

Update component lifecycle methods from `mount()` to `onMount()`:

```javascript
// Before (2.1)
function mount(params = {}) {
    // initialization code
}

// After (2.2)
function onMount(params = {}) {
    // initialization code
}
```

This change provides better consistency with other lifecycle methods and clearer naming conventions.
