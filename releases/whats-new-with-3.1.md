# What's New With 3.1

09/25/2023

## New Features

* Auto-include CBWIRE styles/scripts (no more `wireScripts()` / `wireStyles()`)
* Add `onUpdate()` and `onUpdateProperty()` lifecycle hooks
* Support refreshing all child components from parent actions
* Allow calling CBWIRE UDFs from templates (beyond computed props)
* Fix SFC file name collisions in high-traffic apps
* Clear compiled SFCs on `fwreinit`
* Support `params` arg as alias for `parameters` in `onMount()`
* Add `cacheSingleFileComponents` setting
* Clean up unnecessary template variables
* Allow calling `ColdBox.cfc` and module helpers in templates
* Add `resetExcept()` method

## Bugs

* Fix child components not re-rendering on follow-up requests
* Fix HTML comments in templates breaking re-renders
