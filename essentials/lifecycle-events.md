# Lifecycle Events

## Component Wiring

![](<../.gitbook/assets/image (4).png>)

## mount( event, rc, prc, parameters )

Runs **once** when the component is initially wired.

```javascript
component extends="cbwire.models.Component" {

    function mount( event, rc, prc, parameters ){
        variables.data.incomingValue = event.getValue( "someField" );
    }

}
```

## &#x20;
