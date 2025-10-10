# What's New With 3.0

05/18/2023

## New Features

### Single File Components

Combine component logic and template in a single file using `@startWire` and `@endWire` markers.

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
    // @endWire
</cfscript>

<cfoutput>
<div>
    <h2>Counter: #counter#</h2>
    <button wire:click="increment">+</button>
</div>
</cfoutput>
```

### Module-Aware Components

Components can now be organized within ColdBox modules.

```html
<!-- Load component from specific module -->
#wire("ProductCard@shop", { "productId": 123 })#
#wire("cards.ProductCard@shop", { "productId": 123 })#
```

## Enhancements

### Removed cbValidation Dependency

CBWIRE no longer requires cbValidation, making it more lightweight.

### Simplified Property Access

Templates now use direct property access instead of the `args` scope:

```html
<!-- Old (2.x) -->
<cfoutput>#args.name# - #args.computed.getFullName()#</cfoutput>

<!-- New (3.0) -->
<cfoutput>#name# - #getFullName()#</cfoutput>
```

### Performance Improvements

* Skip unnecessary ColdBox view caching/lookups
* Streamlined engine architecture
* Removed `computedPropertiesProxy` for better performance

## Bug Fixes

* Fixed empty string/null values not passing to Livewire
* Support full-path wire resolution (e.g., `appMapping.wires.SomeComponent`)
* Ensure data-updating actions trigger DOM updates
* `.see()` / `.dontSee()` in tests now chain correctly
* Fix computed property overrides in component tests

## Breaking Changes

### Property Access

```html
<!-- Before -->
<cfoutput>#args.name#</cfoutput>
<!-- After -->
<cfoutput>#name#</cfoutput>
```

### Computed Properties

```html
<!-- Before -->
<cfoutput>#args.computed.getDisplayName()#</cfoutput>
<!-- After -->
<cfoutput>#getDisplayName()#</cfoutput>
```

### Validation Constraints

```javascript
// Before
this.constraints = { "email" = { "required" = true } };
// After
constraints = { "email" = { "required" = true } };
```

### Event Emission

```javascript
// Before
emitTo("event-name", "ComponentName");
// After
emitTo("ComponentName", "event-name");
```
