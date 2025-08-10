# Validation

[Wires](../essentials/creating-components.md) include powerful validations that you can use to validate your [Data Properties](../essentials/properties.md).

{% hint style="info" %}
Validations are provided by [cbValidation](https://coldbox-validation.ortusbooks.com/).
{% endhint %}

## Defining Constraints

You can define constraints within your wires using `this.constraints`.

```javascript
component extends="cbwire.models.Component"{

    this.constraints = {
        "task": { required: true }
    };
    
    // Data Properties
    data = {
        "task": ""
    };
}
```

## Validating

[Actions](../essentials/actions.md) can validate against the defined constraints using `validate()` or `validateOrFail()`.

### validate

Returns a `ValidateResult` object.

```javascript
component extends="cbwire.models.Component"{
    // Action
    function addTask() {
        var result = validate(); // ValidateResult object
        
        if ( !result.hasErrors() ) {
            queryExecute( ... );
        }
    }
}
```

### validateOrFail

Silently fails and prevents further processing of the current [Action](../essentials/actions.md).&#x20;

```javascript
component extends="cbwire.models.Component"{
    // Action
    function addTask() {
        validateOrFail();

        queryExecute( ... );        
    }
}
```

{% hint style="info" %}
With `validateOrFail(),`the error is gracefully caught and any further processing of the invoked action is prevented. The XHR response and re-rendered template are still returned. The actual errors themselves are available to the template using `args.validation`.&#x20;



If you need more granular control over the validation response, use `validate()` instead.
{% endhint %}

## Validation Manager

You can get a new _ValidationManager_ object to work with, just call `getValidationManager()`;&#x20;

```javascript
component extends="cbwire.models.Component"{
    // Action
    function addTask() {
        var data = { "email": "cbwire@rocks.com" };

        var validationManager = getValidationManager();
        
        var result = validationManager.validate(
            target=data,
            constraints={
                "email": { required: true, type: "email" }
            }
        );
    }
}
```

## Displaying Errors

[Templates](../essentials/templates.md) can access the _ValidationResults_ object using `args.validation`. This includes helpful methods you can use for displaying error messages.

```html
<cfoutput>
<div>
    <input wire:model="task" type="text">
    <button wire:click="addTask">Add</button>
    
    <cfif args.validation.hasErrors( "task" )>
        <cfloop array="#args.validation.getAllErrors( "task" )#" index="error">
            <div>#error#</div>
        </cfloop>
    </cfif>
</div>
</cfoutput>
```
