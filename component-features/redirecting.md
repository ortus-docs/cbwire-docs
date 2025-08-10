# Redirecting

You can redirect users from your cbwire components similar to how you would in ColdBox using `relocate`.

```javascript
// File: ./wires/FindADeveloper.cfc

component extends="cbwire.models.Component"{

    function findADev(){
        return this.relocate( URL="https://www.ortussolutions.com" );
    }
    
    function renderIt(){
        return this.renderView( "wires/findADev" );
    }
}

// File: ./views/wires/findADev.cfm

<cfoutput>
<div>
    <button wire:click="findADev">Help mah code!</button>
</div>
</cfoutput>
```

In fact, cbwire uses ColdBox's internal `relocate()` method, so any of the arguments that ColdBox accepts can also be used with cbwire.

```javascript
/**
 * Relocate user browser requests to other events, URLs, or URIs.
 *
 * @event The name of the event to run, if not passed, then it will use the default event found in your configuration file
 * @URL The full URL you would like to relocate to instead of an event: ex: URL='http://www.google.com'
 * @URI The relative URI you would like to relocate to instead of an event: ex: URI='/mypath/awesome/here'
 * @queryString The query string or struct to append, if needed. If in SES mode it will be translated to convention name value pairs
 * @persist What request collection keys to persist in flash ram
 * @persistStruct A structure key-value pairs to persist in flash ram
 * @addToken Wether to add the tokens or not. Default is false
 * @ssl Whether to relocate in SSL or not
 * @baseURL Use this baseURL instead of the index.cfm that is used by default. You can use this for ssl or any full base url you would like to use. Ex: https://mysite.com/index.cfm
 * @postProcessExempt Do not fire the postProcess interceptors
 * @statusCode The status code to use in the relocation
 */
void function relocate(
    event,
    URL,
    URI,
    queryString,
    persist,
    struct persistStruct,
    boolean addToken,
    boolean ssl,
    baseURL,
    boolean postProcessExempt,
    numeric statusCode
);
```
