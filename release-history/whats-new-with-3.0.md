# What's New With 3.0

## 05/18/2023

### Enhancements

[CBWIRE-104](https://github.com/coldbox-modules/cbwire/issues/104): Add Inline Components ( NEW SYNTAX )

[CBWIRE-109](https://github.com/coldbox-modules/cbwire/issues/109): Make components module-aware and allow components to be created within modules

### Changed

[CBWIRE-93](https://github.com/coldbox-modules/cbwire/issues/93): Remove cbValidation as a dependency of CBWIRE

[CBWIRE-103](https://github.com/coldbox-modules/cbwire/issues/103): Increase component rendering performance by bypassing unnecessary ColdBox view caching and lookups

[CBWIRE-105](https://github.com/coldbox-modules/cbwire/issues/105): Refactor core engine for better performance and easier maintenance

[CBWIRE-106](https://github.com/coldbox-modules/cbwire/issues/106):  Remove computedPropertiesProxy as it's no longer needed

[CBWIRE-107](https://github.com/coldbox-modules/cbwire/issues/107): Replace calling computed properties in templates using #args.computed.\[property]\()# with #property()# instead.

[CBWIRE-108](https://github.com/coldbox-modules/cbwire/issues/108): Replace calling data properties in templates using #args.\[property]\()# with #property# instead.

[CBWIRE-110](https://github.com/coldbox-modules/cbwire/issues/110): Using new component format, validation references this.constraints. Change to just 'constraints'.&#x20;

[CBWIRE-113](https://github.com/coldbox-modules/cbwire/issues/113): Change incorrect emitTo( event, component ) signature to instead by emitTo( component, event )

### Bug Fixes

[CBWIRE-97](https://github.com/coldbox-modules/cbwire/issues/97): Empty string and null values are not being properly passed to Livewire

[CBWIRE-100](https://github.com/coldbox-modules/cbwire/issues/100): Fix ability to locate Wires using full path such as appMapping.wires.SomeComponent

[CBWIRE-102](https://github.com/coldbox-modules/cbwire/issues/102): Component actions that update data properties don't always update the DOM

[CBWIRE-111](https://github.com/coldbox-modules/cbwire/issues/111): Calling .see() and .dontSee() in component tests doesn't return the test object for additional method chaining

[CBWIRE-112](https://github.com/coldbox-modules/cbwire/issues/112): Overriding computed properties not working when writing component tests

