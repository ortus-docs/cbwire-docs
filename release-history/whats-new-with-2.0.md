# What's New With 2.0

## 08/30/2022

### Added

CBWIRE-24: File Uploads

CBWIRE-55: Ability to write unit tests for components

CBWIRE-59: Ability to override the default 'wires' folder location

CBWIRE-60: noRender() method to prevent template rendering during actions

CBWIRE-61: Implement dirty property tracking

CBWIRE-65: Invoke computed properties during template rendering and only execute once per request

CBWIRE-70: Ability to specify a 'moduleRootURI' setting to change the URI path for cbwire

CBWIRE-71: Upgrade Livewire JS to v2.10.6

CBWIRE-81: Option to specify the component's template path using this.template instead of defining renderIt() method.

CBWIRE-82: Disable browser caching on XHR responses

CBWIRE-83: Reduce payload bloat by removing unnecessary data in XHR requests and responses

CBWIRE-84: Move internal methods to a separate Engine object to avoid collisions with user-defined methods

CBWIRE-85: Reject incoming XHR request if 'X-Livewire' HTTP Header is not present

CBWIRE-86: Specify null values for data properties

CBWIRE-87: Dependency injection capabilities to cbwire components

CBWIRE-90: Update to match Livewire's current incoming and outgoing HTTP responses

CBWIRE-91: Ability to use Turbo to create Single-page Applications ( SPAs )

### Fixed

CBWIRE-73: Calling reset( "someProperty" ) throws error

CBWIRE-74: Browser back history doesn't work

CBWIRE-80: On subsequent renderings of components, it's changing the unique id and causing DOM diff issues

CBWIRE-88: Livewire expects params to be an array

CBWIRE-89: Not passing parameters when calling update methods
