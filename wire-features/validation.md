---
description: Powerful validations for your forms and input elements.
---

# Validation

CBWIRE includes powerful validations provided by [cbValidation](https://coldbox-validation.ortusbooks.com/) that you can use to validate your [Properties](../essentials/properties.md).

## Installation

You'll need to install cbValidation to use the validation features with CBWIRE.

To install cbValidation, run the following command in CommandBox:

```
box install cbvalidation
```

{% hint style="info" %}
Previous versions of CBWIRE included cbValidation automatically. However, this introduced issues if you wanted to use a module version different from CBWIRE's version, so you now must manually install cbValidation.
{% endhint %}

## Defining Constraints

You can define constraints within your components by populating a _constraints_ struct. Once constraints are defined, CBWIRE will automatically validate your component on each request.

<pre class="language-html"><code class="lang-html">&#x3C;cfscript>
    constraints = {
        "task": { required: true }
    };
    
    // Data Properties
    data = {
        "task": ""
    };
&#x3C;/cfscript>

<strong>&#x3C;cfoutput>
</strong>    &#x3C;div>
        &#x3C;!-- HTML goes here -->
    &#x3C;/div>
&#x3C;/cfoutput>
</code></pre>

## Manual Validation

You can manually validate your component using the _validate()_ or _validateOrFail()_ methods.

### validate

Returns a _ValidateResult_ object.

```html
<cfscript>
    constraints = {
        "task": { required: true }
    };

    // Action
    function addTask() {
        var result = validate(); // ValidateResult object
        
        if ( !result.hasErrors() ) {
            // Do something
        }
    }
</cfscript>

<cfoutput>
    <div>
        <!-- HTML goes here -->
    </div>
</cfoutput>
```

### validateOrFail

Silently fails and prevents further processing of the current action.

```html
<cfscript>
    constraints = {
        "task": { required: true }
    };

    // Action
    function addTask() {
        validateOrFail();
        // Continue if validation passes
    }
</cfscript>

<cfoutput>
    <div>
        <!-- HTML goes here -->
    </div>
</cfoutput>
```

{% hint style="info" %}
With _validateOrFail()_, the error is gracefully caught and any further processing of the invoked action is prevented. The XHR response and re-rendered template are still returned. The actual errors themselves are available to the template using the _validation_ variable.



If you need more granular control over the response, use _validate()_ instead.
{% endhint %}

## Explicit Constraints

If you can also run validations against any constraints you provide by passing them to the _validate()_ method.

```html
<cfscript>
    // Action
    function addTask() {
        var result = validate(
            target=data,
            constraints={
                "task": { required: true }
            }
        ); // ValidateResult object
        
        if ( !result.hasErrors() ) {
            // Do something
        }
    }
</cfscript>

<cfoutput>
    <div>
        <!-- HTML goes here -->
    </div>
</cfoutput>
```

## Displaying Errors

You can access the _ValidationResults_ object using the _validation_ variable. This includes helpful methods you can use for displaying error messages.

```html
<cfscript>
    // ....
</cfscript>

<cfoutput>
<div>
    <input wire:model="task" type="text">
    <button wire:click="addTask">Add</button>
    
    <cfif validation.hasErrors( "task" )>
        <cfloop array="#validation.getAllErrors( "task" )#" index="error">
            <div>#error#</div>
        </cfloop>
    </cfif>
</div>
</cfoutput>
```
