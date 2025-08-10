# What's New With 3.0

05/18/2023

## New Features

* Add Inline Components (new syntax)
* Support module-aware components (create inside modules)

## Enhancements

* Drop `cbValidation` dependency
* Speed up rendering by skipping ColdBox view caching/lookups
* Refactor engine for performance and maintainability
* Remove `computedPropertiesProxy`
* Use `#property()#` instead of `#args.computed.property()#`
* Use `#property#` instead of `#args.property()#`
* Change `this.constraints` to `constraints` in new format
* Fix `emitTo(event, component)` → should be `emitTo(component, event)`

## Bugs

* Fix empty string/null values not passing to Livewire
* Support full-path wire resolution (e.g., `appMapping.wires.SomeComponent`)
* Ensure data-updating actions trigger DOM updates
* `.see()` / `.dontSee()` in tests now chain correctly
* Fix computed property overrides in component tests

