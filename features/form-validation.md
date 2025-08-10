# Form Validation

## Overview

You can use ColdBox's validation engine [cbValidation](https://coldbox-validation.ortusbooks.com/) and [**wire:model**](../template-directives/wire-model.md) to validate your forms.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Form.bx
class extends="cbwire.models.Component" {
    // Data properties
    data = {
        "task": ""
    };
    // Validation constraints
    constraints = {
        "task": {
            "required": true,
            "requiredMessage": "Task is required"
        }
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Form.cfc
component extends="cbwire.models.Component" {
    // Data properties
    data = {
        "task": ""
    };
    // Validation constraints
    constraints = {
        "task": {
            "required": true,
            "requiredMessage": "Task is required"
        }
    };
}
```
{% endtab %}
{% endtabs %}

## Installation

To install cbValidation, run the following command in [CommandBox](https://www.ortussolutions.com/products/commandbox):

```
box install cbvalidation
```

{% hint style="info" %}
Previous versions of CBWIRE included cbValidation automatically. However, this introduced issues if you wanted to use a version different from CBWIRE's.
{% endhint %}

## Defining Constraints

You can define constraints within your [components](../the-essentials/components.md) by populating a **constraints** struct. Once constraints are defined, CBWIRE will automatically validate your component on each request.

```javascript
// Data properties
data = {
    "task": ""
};

// Validation constraints
constraints = {
    "task": {
        "required": true,
        "requiredMessage": "Task is required"
    }
};
```

## Available Constraints

Below are the currently available constraints.&#x20;

{% hint style="info" %}
Review the [cbValidation documentation](https://coldbox-validation.ortusbooks.com/) for additional information.
{% endhint %}

```javascript
propertyName = {
        // The field under validation must be yes, on, 1, or true. This is useful for validating "Terms of Service" acceptance.
        accepted : any value
        
        // The field under validation must be a date after the set targetDate
        after : targetDate
        
        // The field under validation must be a date after or equal the set targetDate
        afterOrEqual : targetDate

        // The field must be alpha ONLY
        alpha : any value
        
        // The field under validation is an array and all items must pass this validation as well
        arrayItem : {
            // All the constraints to validate the items with
        }
        
        // The field under validation must be a date before the set targetDate
        before : targetDate
        
        // The field under validation must be a date before or equal the set targetDate
        beforeOrEqual : targetDate
        
        // The field under validation is a struct and all nested validation rules must pass
        constraints: {
           // All the constraints for the nested struct
        }
        
        // The field under validation must be a date that is equal the set targetDate
        dateEquals : targetDate
        
        // discrete math modifiers
        discrete : (gt,gte,lt,lte,eq,neq):value
        
        // the field must or must not be an empty value
        // needed because `required` counts empty strings as valid
        // and `type` ignores empty strings as "not required"
        empty : boolean [false]

        // value in list
        inList : list
        
        // Verify the instance of the target
        InstanceOf : "instance.path"
        
        // An alias for arrayItem
        items : {
            // All the constraints to validate the items with
        }

        // max value
        max : value

        // Validation method to use in the target object must return boolean accept the incoming value and target object 
        method : methodName

        // min value
        min : value
        
        // An alias for constraints
        nestedConstraints: {
           // All the constraints for the nested struct
        }
        
        // not same as but with no case
        notSameAsNoCase : propertyName

        // not same as another property
        notSameAs : propertyName

        // range is a range of values the property value should exist in
        range : eg: 1..10 or 5..-5

        // regex validation
        regex : valid no case regex

        // required field or not, includes null values
        required : boolean [false]

        // The field under validation must be present and not empty if the `anotherfield` field is equal to the passed `value`.
        requiredIf : {
            anotherfield:value, anotherfield:value
        }

        // The field under validation must be present and not empty unless the `anotherfield` field is equal to the passed 
        requiredUnless : {
            anotherfield:value, anotherfield:value
        }

        // same as but with no case
        sameAsNoCase : propertyName

        // same as another property
        sameAs : propertyName

        // size or length of the value which can be a (struct,string,array,query)
        size  : numeric or range, eg: 10 or 6..8

        // specific type constraint, one in the list.
        type  : (alpha,array,binary,boolean,component,creditcard,date,email,eurodate,float,GUID,integer,ipaddress,json,numeric,query,ssn,string,struct,telephone,url,usdate,UUID,xml,zipcode),

        // UDF to use for validation, must return boolean accept the incoming value and target object, validate(value,target,metadata):boolean
        udf = variables.UDF or this.UDF or a closure.

        // Check if a column is unique in the database
        unique = {
            table : The table name,
            column : The column to check, defaults to the property field in check
        }

        // Custom validator, must implement coldbox.system.validation.validators.IValidator
        validator : path or wirebox id, example: 'mypath.MyValidator' or 'id:MyValidator'
}

```

## Manual Validation

You can manually validate your [component](../the-essentials/components.md) using the **validate()** or **validateOrFail()** methods.

### validate

{% hint style="info" %}
**validate()** returns a ValidateResult object
{% endhint %}

```javascript
// Data properties
data = {
    "task": ""
};

// Validation constraints
constraints = {
    "task": { "required": true, "requiredMessage": "Task is required" }
};

// Action
function addTask() {
    var result = validate(); // ValidateResult object
    if ( !result.hasErrors() ) {
        // Do something
    }
}
```

### validateOrFail

Silently fails and prevents further processing of the current [action](../the-essentials/actions.md).

```javascript
// Action
function addTask() {
    validateOrFail(); // fail silently if validation doesn't pass.
    // Otherwise, continue
}
```

{% hint style="info" %}
With **validateOrFail()**, the error is gracefully caught and further processing of the action is prevented.&#x20;

If you need more granular control over the response, use **validate()** instead.
{% endhint %}

## Explicit Constraints

You can also run validations against any constraints you provide by passing them to the **validate()** method, which returns a **ValidateResult** object.

```javascript
// Action
function addTask() {
    var result = validate(
        target=data,
        constraints={
            "task": { required: true }
        }
    );
    if ( !result.hasErrors() ) {
        // Do something
    }
}
```

## Displaying Errors

In your [templates](../the-essentials/templates.md), you have several helper methods for displaying errors.

You can use **hasErrors()** and **getErrors()** to check for validation errors that have occurred and to display all errors.

{% tabs %}
{% tab title="BoxLang" %}
```html
<bx:output>
    <div>
        Add Task
        <bx:if hasErrors()>
            <div class="alert alert-danger">
                <ul>
                    <bx:loop array="#getErrors()#" index="error">
                        <li>#error#</li>
                    </bx:loop>
                </ul>
            </div>
        </bx:if>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<cfoutput>
    <div>
        Add Task
        <cfif hasErrors()>
            <div class="alert alert-danger">
                <ul>
                    <cfloop array="#getErrors()#" index="error">
                        <li>#error#</li>
                    </cfloop>
                </ul>
            </div>
        </cfif>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

To check for errors on a specific data property, you can use **hasError( field)** and **getError( field )**

{% tabs %}
{% tab title="BoxLang" %}
```html
<div>
    <input type="text" wire:model.blur="name">
    <bx:if hasError( "name" )>
        <span class="text-danger">#getError( "name" )</span>
    </bx:if>
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<div>
    <input type="text" wire:model.blur="name">
    <cfif hasError( "name" )>
        <span class="text-danger">#getError( "name" )</span>
    </cfif>
</div>
```
{% endtab %}
{% endtabs %}
