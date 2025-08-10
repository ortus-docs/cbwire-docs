# Logging

We inject a [LogBox](https://logbox.ortusbooks.com/) _logger_ into all cbwire Components to provide you with built-in logging.

```javascript
component extends="cbwire.models.Component"{

    function mount(){
        log.info( "mount() called" );
    }

    function someAction(){
        log.debug( "someAction() called" );
    }

}
```

You can also access the LogBox parent object using `getLogBox()`.

```javascript
component extends="cbwire.models.Component"{

    function mount(){
        var logbox = getLogBox();
        var logger = logbox.getRootLogger();
        log.info( "Logging with the root logger!" );
    }

}
```
