---
description: ColdBox's dependency injection framework, right there when you need it.
---

# WireBox

CBWIRE includes WireBox for pulling any dependencies you may need to use in your components, such as service objects, settings, and more.

## getInstance()

You can pull in dependencies to your component using **getInstance()**.

```html
<cfscript>
    function log() {
        var storage = getInstance( "cbfs:disks:temp" );
        storage.create( "log.txt", "CBWIRE rocks!" );
    }
</cfscript>

<cfoutput>
    <div>
        <!--- HTML goes here --->
    </div>
</cfoutput>
```

## Property Injection

You can also pull in dependencies using property injection.

```html
<cfscript>
    property name="storage" inject="cbfs:disks:temp";

    function log() {
        storage.create( "log.txt", "CBWIRE rocks!" );
    }
</cfscript>

<cfoutput>
    <div>
        <!--- HTML goes here --->
    </div>
</cfoutput>
```

## onDIComplete

If you want to act immediately after WireBox has fully constructor your component with it's dependencies, you can hook into the _onDIComplete()_ method.

```html
<cfscript>
    property name="storage" inject="cbfs:disks:temp";

    function onDIComplete() {
        log.info( "CBWIRE rocks!" );
    }
</cfscript>

<cfoutput>
    <div>
        <!--- HTML goes here --->
    </div>
</cfoutput>
```
