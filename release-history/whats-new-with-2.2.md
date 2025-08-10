# What's New With 2.2

## 01/09/2022

### Added

[CBWIRE-99](https://ortussolutions.atlassian.net/browse/CBWIRE-99): Implement lifecycle hook onHydrate().

[CBWIRE-100](https://ortussolutions.atlassian.net/browse/CBWIRE-100): Implement lifecycle hook onHydrate\[Property]\().

[CBWIRE-103](https://ortussolutions.atlassian.net/browse/CBWIRE-103): Implement an automatic trim() for all data properties.

[CBWIRE-124](https://ortussolutions.atlassian.net/browse/CBWIRE-124): Implement the ability to interact with CBWIRE component from JavaScript using cbwire.find( '#args.\_id#' ).

[CBWIRE-125](https://ortussolutions.atlassian.net/browse/CBWIRE-125): Add configuration setting 'enableTurbo' to automatically include everything needed to work with Turbo for single page applications.

[CBWIRE-130](https://ortussolutions.atlassian.net/browse/CBWIRE-130): Add ability to call reset() without passing a key to reset all data properties to their original values.

### Changed

[CBWIRE-93](https://ortussolutions.atlassian.net/browse/CBWIRE-93): Implement onMount() method instead of mount().

### Fixed

[CBWIRE-121](https://ortussolutions.atlassian.net/browse/CBWIRE-121): DocBox generated docs are failing because of file structure.

[CBWIRE-126](https://ortussolutions.atlassian.net/browse/CBWIRE-126): Listeners are being fired immediately when calling emit() when the listener is defined on the same component, which they shouldn't.

[CBWIRE-127](https://ortussolutions.atlassian.net/browse/CBWIRE-127): onHydrate() is firing after actions are performed.

[CBWIRE-129](https://ortussolutions.atlassian.net/browse/CBWIRE-129): Computed properties are not being rendered before actions are called.
