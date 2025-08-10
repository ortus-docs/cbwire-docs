---
description: Similar to ColdBox, you can relocate users using relocate.
---

# Redirecting

You can redirect users in your [Actions](../essentials/actions.md) using `relocate()`.

## relocate

Redirects a user to a different URI, URL, or event.

```javascript
component extends="cbwire.models.Component"{

    // Action
    function clickButton() {
        // Relocate to Google
        relocate( url="https://www.google.com" );
        
        // Relocate using URI
        relocate( uri="/some/relative/path" );
        
        // Relocate using event and set variables in Flash Ram
        relocate( event="example.index", persistStruct={
            success: true
        } );
    }

}
```

CBWIRE's relocate() method signature is nearly identical to ColdBox's internal relocate() method, with the exception that status codes cannot be set. Otherwise, most of the arguments that ColdBox accepts can be used.

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
