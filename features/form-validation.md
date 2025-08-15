# Form Validation

Validate forms using ColdBox's [cbValidation](https://coldbox-validation.ortusbooks.com/) engine. CBWIRE automatically validates components on each request when constraints are defined, and provides helper methods for displaying validation errors.

## Installation

Install cbValidation via CommandBox:

```bash
box install cbvalidation
```

## Basic Usage

Create a user registration form with validation:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserRegistration.bx
class extends="cbwire.models.Component" {
    data = {
        "name": "",
        "email": "",
        "password": "",
        "confirmPassword": ""
    };
    
    constraints = {
        "name": {
            "required": true,
            "requiredMessage": "Name is required",
            "size": "2..50"
        },
        "email": {
            "required": true,
            "requiredMessage": "Email is required",
            "type": "email"
        },
        "password": {
            "required": true,
            "requiredMessage": "Password is required",
            "size": "8..50"
        },
        "confirmPassword": {
            "required": true,
            "sameAs": "password",
            "sameAsMessage": "Passwords must match"
        }
    };
    
    function register() {
        validateOrFail();
        
        // Save user data
        // redirect("/dashboard");
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserRegistration.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "",
        "email" = "",
        "password" = "",
        "confirmPassword" = ""
    };
    
    constraints = {
        "name" = {
            "required" = true,
            "requiredMessage" = "Name is required",
            "size" = "2..50"
        },
        "email" = {
            "required" = true,
            "requiredMessage" = "Email is required",
            "type" = "email"
        },
        "password" = {
            "required" = true,
            "requiredMessage" = "Password is required",
            "size" = "8..50"
        },
        "confirmPassword" = {
            "required" = true,
            "sameAs" = "password",
            "sameAsMessage" = "Passwords must match"
        }
    };
    
    function register() {
        validateOrFail();
        
        // Save user data
        // redirect("/dashboard");
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userRegistration.bxm -->
<bx:output>
<div>
    <h1>User Registration</h1>
    
    <bx:if hasErrors()>
        <div class="alert alert-danger">
            <h4>Please fix the following errors:</h4>
            <ul>
                <bx:loop array="#getErrors()#" item="error">
                    <li>#error#</li>
                </bx:loop>
            </ul>
        </div>
    </bx:if>
    
    <form wire:submit.prevent="register">
        <div class="form-group">
            <label for="name">Name:</label>
            <input type="text" 
                   id="name" 
                   wire:model.blur="name" 
                   class="form-control #hasError('name') ? 'is-invalid' : ''#">
            <bx:if hasError("name")>
                <span class="invalid-feedback">#getError("name")#</span>
            </bx:if>
        </div>
        
        <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" 
                   id="email" 
                   wire:model.blur="email" 
                   class="form-control #hasError('email') ? 'is-invalid' : ''#">
            <bx:if hasError("email")>
                <span class="invalid-feedback">#getError("email")#</span>
            </bx:if>
        </div>
        
        <div class="form-group">
            <label for="password">Password:</label>
            <input type="password" 
                   id="password" 
                   wire:model.blur="password" 
                   class="form-control #hasError('password') ? 'is-invalid' : ''#">
            <bx:if hasError("password")>
                <span class="invalid-feedback">#getError("password")#</span>
            </bx:if>
        </div>
        
        <div class="form-group">
            <label for="confirmPassword">Confirm Password:</label>
            <input type="password" 
                   id="confirmPassword" 
                   wire:model.blur="confirmPassword" 
                   class="form-control #hasError('confirmPassword') ? 'is-invalid' : ''#">
            <bx:if hasError("confirmPassword")>
                <span class="invalid-feedback">#getError("confirmPassword")#</span>
            </bx:if>
        </div>
        
        <button type="submit" class="btn btn-primary">Register</button>
    </form>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userRegistration.cfm -->
<cfoutput>
<div>
    <h1>User Registration</h1>
    
    <cfif hasErrors()>
        <div class="alert alert-danger">
            <h4>Please fix the following errors:</h4>
            <ul>
                <cfloop array="#getErrors()#" item="error">
                    <li>#error#</li>
                </cfloop>
            </ul>
        </div>
    </cfif>
    
    <form wire:submit.prevent="register">
        <div class="form-group">
            <label for="name">Name:</label>
            <input type="text" 
                   id="name" 
                   wire:model.blur="name" 
                   class="form-control #hasError('name') ? 'is-invalid' : ''#">
            <cfif hasError("name")>
                <span class="invalid-feedback">#getError("name")#</span>
            </cfif>
        </div>
        
        <div class="form-group">
            <label for="email">Email:</label>
            <input type="email" 
                   id="email" 
                   wire:model.blur="email" 
                   class="form-control #hasError('email') ? 'is-invalid' : ''#">
            <cfif hasError("email")>
                <span class="invalid-feedback">#getError("email")#</span>
            </cfif>
        </div>
        
        <div class="form-group">
            <label for="password">Password:</label>
            <input type="password" 
                   id="password" 
                   wire:model.blur="password" 
                   class="form-control #hasError('password') ? 'is-invalid' : ''#">
            <cfif hasError("password")>
                <span class="invalid-feedback">#getError("password")#</span>
            </cfif>
        </div>
        
        <div class="form-group">
            <label for="confirmPassword">Confirm Password:</label>
            <input type="password" 
                   id="confirmPassword" 
                   wire:model.blur="confirmPassword" 
                   class="form-control #hasError('confirmPassword') ? 'is-invalid' : ''#">
            <cfif hasError("confirmPassword")>
                <span class="invalid-feedback">#getError("confirmPassword")#</span>
            </cfif>
        </div>
        
        <button type="submit" class="btn btn-primary">Register</button>
    </form>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Validation Methods

### validateOrFail()

Automatically validates and prevents further action execution if validation fails:

```javascript
function register() {
    validateOrFail(); // Stops execution if validation fails
    
    // Save user data
    redirect("/dashboard");
}
```

### validate()

Returns a ValidationResult object for manual error handling:

```javascript
function register() {
    var result = validate();
    
    if (!result.hasErrors()) {
        // Save user data
        redirect("/dashboard");
    } else {
        // Handle errors manually if needed
    }
}
```

## Error Display Methods

### All Errors

Display all validation errors:

```javascript
hasErrors()    // Returns true if any validation errors exist
getErrors()    // Returns array of all error messages
```

### Field-Specific Errors

Display errors for specific fields:

```javascript
hasError("fieldName")    // Returns true if field has errors
getError("fieldName")    // Returns error message for field
```

## Common Constraints

Most frequently used validation constraints:

| Constraint | Description | Example |
| ---------- | ----------- | ------- |
| `required` | Field must have a value | `{"required": true}` |
| `type` | Field must be specific type | `{"type": "email"}` |
| `size` | String length or numeric range | `{"size": "8..50"}` |
| `min/max` | Minimum/maximum value | `{"min": 1, "max": 100}` |
| `sameAs` | Must match another field | `{"sameAs": "password"}` |
| `regex` | Must match regular expression | `{"regex": "^[A-Z].*"}` |
| `inList` | Value must be in list | `{"inList": "red,blue,green"}` |

## Advanced Validation

### Custom Constraints

Define validation constraints inline:

```javascript
function processForm() {
    var result = validate(
        target = data,
        constraints = {
            "email": {"required": true, "type": "email"},
            "age": {"required": true, "min": 18}
        }
    );
    
    if (!result.hasErrors()) {
        // Process form
    }
}
```

### Conditional Validation

Use `requiredIf` and `requiredUnless` for conditional validation:

```javascript
constraints = {
    "phone": {
        "requiredIf": {"contactMethod": "phone"},
        "type": "telephone"
    },
    "email": {
        "requiredUnless": {"contactMethod": "phone"},
        "type": "email"
    }
};
```

{% hint style="info" %}
CBWIRE automatically validates components when constraints are defined. Validation runs on each request before actions execute.
{% endhint %}

{% hint style="warning" %}
For complete constraint documentation, see [cbValidation documentation](https://coldbox-validation.ortusbooks.com/).
{% endhint %}
