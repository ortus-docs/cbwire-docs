# What's New With 3.0

05/18/2023

## New Features

### Inline Components (Single File Components)

CBWIRE 3.0 introduces a new inline component syntax that allows you to define both template and component logic in a single file.

```html
<!-- wires/Counter.cfm -->
<cfscript>
    // @startWire
    data = {
        "counter" = 0
    };
    
    function increment() {
        data.counter += 1;
    }
    
    function decrement() {
        data.counter -= 1;
    }
    
    function reset() {
        data.counter = 0;
    }
    // @endWire
</cfscript>

<cfoutput>
<div>
    <h2>Counter: #counter#</h2>
    <button wire:click="increment">+</button>
    <button wire:click="decrement">-</button>
    <button wire:click="reset">Reset</button>
</div>
</cfoutput>
```

This new syntax eliminates the need for separate `.cfc` and `.cfm` files for simple components.

### Module-Aware Components

Components can now be created and organized within ColdBox modules, improving application structure and modularity.

```javascript
// modules/shop/wires/ProductCard.cfc
component extends="cbwire.models.Component" {
    data = {
        "product" = {},
        "inCart" = false
    };
    
    function onMount(params = {}) {
        if (structKeyExists(params, "productId")) {
            data.product = getProduct(params.productId);
            data.inCart = isInCart(params.productId);
        }
    }
    
    function addToCart() {
        addProductToCart(data.product.id);
        data.inCart = true;
    }
}
```

```html
<!-- Load component from module -->
#wire("ProductCard@shop", { "productId": 123 })#

<!-- Components can also be nested in module folders -->
#wire("cards.ProductCard@shop", { "productId": 123 })#
```

## Enhancements

### Removed cbValidation Dependency

CBWIRE 3.0 drops the required dependency on `cbValidation`, making the module more lightweight and flexible.

```javascript
// Old way (2.x) - required cbValidation
this.constraints = {
    "email" = { "required" = true, "type" = "email" }
};

// New way (3.0) - constraints without cbValidation requirement
constraints = {
    "email" = { "required" = true, "type" = "email" }
};
```

### Performance Optimizations

Significant rendering performance improvements by optimizing ColdBox integration:

- Skip unnecessary ColdBox view caching and lookups
- Streamlined engine architecture for better maintainability
- Faster component initialization and rendering

### Simplified Property Access

Updated property access syntax for cleaner, more intuitive templates:

```html
<!-- Old syntax (2.x) -->
<cfoutput>
<div>
    #args.computed.fullName()#
    #args.email#
</div>
</cfoutput>

<!-- New syntax (3.0) -->
<cfoutput>
<div>
    #fullName()#
    #email#
</div>
</cfoutput>
```

### Computed Properties Proxy Removal

Eliminated the `computedPropertiesProxy` for better performance and simpler debugging:

```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    data = {
        "firstName" = "John",
        "lastName" = "Doe"
    };
    
    // Computed properties now accessed directly
    function getFullName() {
        return data.firstName & " " & data.lastName;
    }
}
```

```html
<!-- Template access -->
<cfoutput>
<h1>Welcome #getFullName()#!</h1>
</cfoutput>
```

### Improved Event Emission

Fixed event emission syntax for better consistency:

```javascript
// wires/NotificationPanel.cfc
component extends="cbwire.models.Component" {
    function sendAlert() {
        // Old syntax (incorrect)
        // emitTo("alert-received", "AlertHandler");
        
        // New syntax (correct)
        emitTo("AlertHandler", "alert-received");
    }
}
```

## Bug Fixes

### Data Handling Improvements

- **Fixed empty string/null values**: Empty strings and null values now properly pass to Livewire
- **Enhanced data-updating actions**: Actions that update data properties now correctly trigger DOM updates

### Component Resolution

- **Full-path wire resolution**: Support for full application mapping paths like `appMapping.wires.SomeComponent`
- **Better wire location**: Improved component discovery and loading

### Testing Enhancements

- **Chainable test methods**: `.see()` and `.dontSee()` test methods now chain correctly
- **Fixed computed property overrides**: Computed properties can now be properly overridden in component tests

```javascript
// TestCase example
function testUserProfile() {
    var comp = visitComponent("UserProfile")
        .set("firstName", "Jane")
        .set("lastName", "Smith")
        .call("updateProfile");
        
    comp.see("Jane Smith")
        .dontSee("John Doe");
}
```

## Breaking Changes

### Property Access Syntax

Update template property access from `#args.property#` to `#property#`:

```html
<!-- Before (2.x) -->
<cfoutput>
<div>Name: #args.name#</div>
<div>Email: #args.email#</div>
</cfoutput>

<!-- After (3.0) -->
<cfoutput>
<div>Name: #name#</div>
<div>Email: #email#</div>
</cfoutput>
```

### Computed Properties

Change computed property access from `#args.computed.method()#` to `#method()#`:

```html
<!-- Before (2.x) -->
<cfoutput>
<h1>#args.computed.getDisplayName()#</h1>
</cfoutput>

<!-- After (3.0) -->
<cfoutput>
<h1>#getDisplayName()#</h1>
</cfoutput>
```

### Validation Constraints

Move validation constraints from `this.constraints` to `constraints`:

```javascript
// Before (2.x)
component extends="cbwire.models.Component" {
    this.constraints = {
        "email" = { "required" = true }
    };
}

// After (3.0)
component extends="cbwire.models.Component" {
    constraints = {
        "email" = { "required" = true }
    };
}
```

### Event Emission

Update `emitTo()` method parameter order:

```javascript
// Before (2.x) - incorrect parameter order
emitTo("event-name", "ComponentName");

// After (3.0) - correct parameter order
emitTo("ComponentName", "event-name");
```

