# What's New With 2.0



08/30/2022

## New Features

* File uploads support
* Write unit tests for components
* Override default `wires` folder location
* `noRender()` method to skip template rendering
* Track dirty properties
* Run computed properties once per request during rendering
* Add `moduleRootURI` setting to customize URI path
* Upgrade Livewire JS to v2.10.6
* Disable browser caching for XHR responses
* Trim XHR payload by removing unnecessary data
* Move internal methods to Engine object to prevent conflicts
* Reject XHR requests without `X-Livewire` header
* Support dependency injection in components
* Match Livewire's HTTP request/response behavior
* Enable Turbo support for SPAs

## Enhancements

* Set component template path using `this.template`
* Allow `null` as a valid data property value

## Bugs

* Fix error when calling `reset("someProperty")`
* Fix browser back button issues
* Preserve component IDs across renders to avoid DOM diffing issues
* Ensure `params` is passed as an array
* Fix missing parameters in update method calls
