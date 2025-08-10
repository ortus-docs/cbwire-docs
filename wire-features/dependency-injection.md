---
description: Just as with ColdBox, you can inject your dependencies using WireBox.
---

# Dependency Injection

## WireBox

[Wires](../essentials/creating-components.md) have [WireBox](https://wirebox.ortusbooks.com/) available for injecting dependencies such as service objects, session storage, you name it!

```javascript
component extends="cbwire.models.Component" {

    // Property injection
    property name="storage" inject="cbfs:disks:temp";

    // Setter injection
    function setStorage() inject="cbfs:disks:temp" {};

    // Actions
    function someAction() {
        // Use the storage object
        storage.create( "log.txt", "CBWIRE rocks!" );
        // Use getInstance() to get objects.
        var defaultStorage = getInstance( "cbfs:disks:default" ); 
    }

}
```

## onDIComplete

If you want to act immediately after dependency injection has been completed, you can define an `onDIComplete` method.

```javascript
component extends="cbwire.models.Component" {

    property name="storage" inject="cbfs:disks:temp";

    function onDIComplete(){
        storage.create( "log.txt", "Dependency Injection Complete!" );
    }

}
```
