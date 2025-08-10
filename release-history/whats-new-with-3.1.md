# What's New With 3.1

## 09/25/2023

### Enhancements

* [CBWIRE 81](https://github.com/coldbox-modules/cbwire/issues/81): Add configuration property to include CBWIRE styling and JavaScript assets automatically. Remove the need to add wireScripts() and wireStyles() to templateBugs
* [CBWIRE 116](https://github.com/coldbox-modules/cbwire/issues/116): Add onUpdate() and onUpdateProperty() lifecycle hooks
* [CBWIRE 117](https://github.com/coldbox-modules/cbwire/issues/117): Add the ability to refresh all child components from the parent component when an action is called
* [CBWIRE 118](https://github.com/coldbox-modules/cbwire/issues/118): Add the ability to call CBWIRE UDFs from the cbwire template as computed properties are too limiting.
* [CBWIRE 119](https://github.com/coldbox-modules/cbwire/issues/119): Single file components file names are not unique and could create conflicts in high-traffic applications
* [CBWIRE 120](https://github.com/coldbox-modules/cbwire/issues/120): Clear any compiled single file components on ColdBox fwreinit
* [CBWIRE 121](https://github.com/coldbox-modules/cbwire/issues/121): Add params arguments as a substitute for parameters argument when calling onMount
* [CBWIRE 123](https://github.com/coldbox-modules/cbwire/issues/123): Add 'cacheSingleFileComponents' setting to control if single file components are cached
* [CBWIRE 126](https://github.com/coldbox-modules/cbwire/issues/126): Cleanup unnecessary variables available to templates to avoid potential collisions
* [CBWIRE 127](https://github.com/coldbox-modules/cbwire/issues/127): Add the ability to call application helpers defined in ColdBox.cfc or any installed modules from CBWIRE templates.
* [CBWIRE 129](https://github.com/coldbox-modules/cbwire/issues/129): Add resetExcept()

### Bug Fixes

* [CBWIRE 124](https://github.com/coldbox-modules/cbwire/issues/124): Fix child components not re-rendering properly on subsequent requests
* [CBWIRE 122](https://github.com/coldbox-modules/cbwire/issues/122): HTML comments in component templates cause re-renders not to work.
