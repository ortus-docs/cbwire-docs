---
description: Methods to redirect your users where you need.
---

# Redirecting

You can redirect users in your actions using **relocate()**.

```html
<cfscript>
    function redirectToURI() {
        return relocate( uri="/some-url" );
    }

    function redirectToURL() {
        return relocate( uri="https://www.google.com" );
    }

    function redirectToEvent(){
        return relocate( event="examples.index" );
    }

    function redirectWithFlash() {
        relocate( event="example.index", persistStruct={
            confirm: "Redirect successful"
        } );
    }
</cfscript>

<cfoutput>
    <div>
        <button wire:click="redirectToURI">Redirect To URI</button>
        <button wire:click="redirectToURL">Redirect To URL</button>
        <button wire:click="redirectToEvent">Redirect To Event</button>
        <button wire:click="redirectWithFlash">Redirect With Flash</button>
    </div>
</cfoutput>
```

## Arguments

CBWIRE's **relocate()** method signature is nearly identical to ColdBox's internal _relocate()_ method, except that status codes cannot be set. Otherwise, most of the arguments that ColdBox accepts can be used.

```javascript
/**
 * Relocate user browser requests to other events, URLs, or URIs.
 *
 * @event             The name of the event to relocate to, if not passed, then it will use the default event found in your configuration file.
 * @queryString       The query string or a struct to append, if needed. If in SES mode it will be translated to convention name value pairs
 * @addToken          Wether to add the tokens or not to the relocation. Default is false
 * @persist           What request collection keys to persist in flash RAM automatically for you
 * @persistStruct     A structure of key-value pairs to persist in flash RAM automatically for you
 * @ssl               Whether to relocate in SSL or not. You need to explicitly say TRUE or FALSE if going out from SSL. If none passed, we look at the even's SES base URL (if in SES mode)
 * @baseURL           Use this baseURL instead of the index.cfm that is used by default. You can use this for SSL or any full base url you would like to use. Ex: https://mysite.com/index.cfm
 * @postProcessExempt Do not fire the postProcess interceptors, by default it does
 * @URL               The full URL you would like to relocate to instead of an event: ex: URL='http://www.google.com'
 * @URI               The relative URI you would like to relocate to instead of an event: ex: URI='/mypath/awesome/here'
 *
 * @return void
 */
function relocate(
	event                = "",
	queryString          = "",
	boolean addToken     = false,
	persist              = "",
	struct persistStruct = structNew()
	boolean ssl,
	baseURL                   = "",
	boolean postProcessExempt = false,
	URL,
	URI
)
```
