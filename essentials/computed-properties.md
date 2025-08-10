---
description: Dynamic, cached, properties that can return any CFML data type.
---

# Computed Properties

Computed Properties are dynamic properties and are helpful for deriving values from a database or another persistent store like a cache.&#x20;

Computed Properties are similar to [Data Properties](properties.md) with some key differences:

* They are declared as inline functions using **computed**_._
* They can return any CFML data type, not just values that can be parsed by JavaScript like [Data Properties](properties.md).

## Defining Properties

You can define Computed Properties on your components using **computed**.&#x20;

```html
<cfscript>
    computed = {
        "tasks": function() {
            return queryExecute( "
                select *
                from tasks
            " );
        }
    };
</cfscript>
```

## Accessing Properties

You can access Computed Properties in your component template using **propertyName()**.

```html
<cfscript>
    computed = {
        "currentTime": function() {
            return now();
        }
    };
</cfscript>

<cfoutput>
    <div>
        Current time: #currentTime()#
    </div>
</cfoutput>
```

You can also access Computed Properties from within your [Actions](computed-properties.md#actions).

```html
<cfscript>
    computed = {
        "currentTime" : function() {
            return now();
        }
    };    
    
    function sendEmail() {
        if ( dateDiff( "d", computed.currentTime() ) > 3 ) {

        }
    }
</cfscript>
```

{% hint style="info" %}
Computed Properties are defined as a CFML closure. That means that to get the result from the function, you must invoke it like so.

```javascript
myComputedProperty() // good
```

```
myComputedProperty // bad
```
{% endhint %}

## Caching

**Computed Properties are cached for the lifetime of the request.** If you reference your Computed Property three times in your component template or from within a component action, it will only execute once.

```html
<cfscript>
    computed = {
        "getUUID": function() {
            return createUUID();
        }
    }
</cfscript>

<cfoutput>
    <div>
        <p>#getUUID()#</p>
        <p>#getUUID()#</p> <!-- Outputs the same ID because of caching -->
    </div>
</cfoutput>
```

You can prevent caching on a computed property by passing a false argument when invoking it.

```html
<p>#getUUID()#</p>
<p>#getUUID( false )#</p> <!-- Does not use caching --->
```
