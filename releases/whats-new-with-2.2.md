# What's New With 2.2

01/09/2022

## New Features

* Add `onHydrate()` and `onHydrate[Property]()` lifecycle hooks
* Auto-trim all data properties
* Allow JS access to components via `cbwire.find('#args._id#')`
* Add `enableTurbo` setting for SPA support
* Allow `reset()` to reset all data without a key

## Enhancements

* Use `onMount()` instead of `mount()`

## Bugs

* Fix DocBox docs breaking due to file structure
* Prevent listeners from firing immediately on same-component `emit()`
* Ensure `onHydrate()` runs before actions
* Fix computed properties not rendering before actions
