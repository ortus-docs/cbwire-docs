# wire:submit

The `wire:submit` directive provides a seamless way to handle form submissions in your CBWIRE components. Instead of dealing with traditional form processing and page refreshes, `wire:submit` intercepts form submissions and calls your component methods directly, creating smooth, dynamic form experiences.

## Basic Usage

Add `wire:submit` to any form element to handle submissions through your component. When users submit the form, CBWIRE will automatically prevent the default browser submission and call your specified method instead.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ContactForm.bx
class extends="cbwire.models.Component" {
    data = {
        "name": "",
        "email": "",
        "message": "",
        "isSubmitting": false
    };

    function submitContact() {
        data.isSubmitting = true;
        
        // Validate the form data
        if (!data.name.trim().len() || !data.email.trim().len()) {
            data.isSubmitting = false;
            return;
        }
        
        // Process the contact form
        contactService = getInstance("ContactService");
        contactService.saveContact({
            "name": data.name,
            "email": data.email, 
            "message": data.message
        });
        
        // Reset form
        data.name = "";
        data.email = "";
        data.message = "";
        data.isSubmitting = false;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ContactForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "",
        "email" = "",
        "message" = "",
        "isSubmitting" = false
    };

    function submitContact() {
        data.isSubmitting = true;
        
        // Validate the form data
        if (!len(trim(data.name)) || !len(trim(data.email))) {
            data.isSubmitting = false;
            return;
        }
        
        // Process the contact form
        contactService = getInstance("ContactService");
        contactService.saveContact({
            "name" = data.name,
            "email" = data.email, 
            "message" = data.message
        });
        
        // Reset form
        data.name = "";
        data.email = "";
        data.message = "";
        data.isSubmitting = false;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/contactForm.bxm -->
<bx:output>
<form wire:submit="submitContact">
    <div>
        <label for="name">Name:</label>
        <input type="text" id="name" wire:model="name" required>
    </div>
    
    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" wire:model="email" required>
    </div>
    
    <div>
        <label for="message">Message:</label>
        <textarea id="message" wire:model="message" rows="4"></textarea>
    </div>
    
    <button type="submit">
        <bx:if isSubmitting>Sending...<bx:else>Send Message</bx:if>
    </button>
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/contactForm.cfm -->
<cfoutput>
<form wire:submit="submitContact">
    <div>
        <label for="name">Name:</label>
        <input type="text" id="name" wire:model="name" required>
    </div>
    
    <div>
        <label for="email">Email:</label>
        <input type="email" id="email" wire:model="email" required>
    </div>
    
    <div>
        <label for="message">Message:</label>
        <textarea id="message" wire:model="message" rows="4"></textarea>
    </div>
    
    <button type="submit">
        <cfif isSubmitting>Sending...<cfelse>Send Message</cfif>
    </button>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## What wire:submit Does

When you add `wire:submit` to a form, CBWIRE automatically:

- **Prevents Default Submission**: Stops the browser from submitting the form traditionally
- **Calls Your Method**: Executes the specified component method when the form is submitted
- **Disables Form Elements**: Automatically disables submit buttons and marks form inputs as readonly during processing
- **Maintains State**: Keeps all your component data intact during the submission process

## Submit Modifiers

Customize form submission behavior with modifiers:

### Prevent Modifier

Use `wire:submit.prevent` to explicitly prevent the default form submission (though this is the default behavior):

{% tabs %}
{% tab title="BoxLang" %}
```html
<bx:output>
<form wire:submit.prevent="saveUser">
    <!-- Form fields -->
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<cfoutput>
<form wire:submit.prevent="saveUser">
    <!-- Form fields -->
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Practical Example

This user registration form demonstrates multiple `wire:submit` features including form handling, validation using cbValidation, error display, loading states, and success handling:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserRegistration.bx
class extends="cbwire.models.Component" {
    data = {
        "firstName": "",
        "lastName": "",
        "email": "",
        "password": "",
        "confirmPassword": "",
        "isRegistering": false
    };
    
    constraints = {
        "firstName": {
            "required": true,
            "requiredMessage": "First name is required"
        },
        "lastName": {
            "required": true,
            "requiredMessage": "Last name is required"
        },
        "email": {
            "required": true,
            "type": "email",
            "requiredMessage": "Email is required",
            "typeMessage": "Please enter a valid email address"
        },
        "password": {
            "required": true,
            "size": "6..",
            "requiredMessage": "Password is required",
            "sizeMessage": "Password must be at least 6 characters"
        },
        "confirmPassword": {
            "required": true,
            "sameAs": "password",
            "requiredMessage": "Please confirm your password",
            "sameAsMessage": "Passwords must match"
        }
    };

    function register() {
        data.isRegistering = true;
        
        // Validate using cbValidation
        if (!validateOrFail()) {
            data.isRegistering = false;
            return;
        }
        
        // Register the user
        userService = getInstance("UserService");
        result = userService.createUser({
            "firstName": data.firstName,
            "lastName": data.lastName,
            "email": data.email,
            "password": data.password
        });
        
        if (result.success) {
            // Redirect to login page
            redirect("/login");
        } else {
            // Handle server-side errors
            data.isRegistering = false;
            return;
        }
        
        data.isRegistering = false;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserRegistration.cfc
component extends="cbwire.models.Component" {
    data = {
        "firstName" = "",
        "lastName" = "",
        "email" = "",
        "password" = "",
        "confirmPassword" = "",
        "isRegistering" = false
    };
    
    constraints = {
        "firstName" = {
            "required" = true,
            "requiredMessage" = "First name is required"
        },
        "lastName" = {
            "required" = true,
            "requiredMessage" = "Last name is required"
        },
        "email" = {
            "required" = true,
            "type" = "email",
            "requiredMessage" = "Email is required",
            "typeMessage" = "Please enter a valid email address"
        },
        "password" = {
            "required" = true,
            "size" = "6..",
            "requiredMessage" = "Password is required",
            "sizeMessage" = "Password must be at least 6 characters"
        },
        "confirmPassword" = {
            "required" = true,
            "sameAs" = "password",
            "requiredMessage" = "Please confirm your password",
            "sameAsMessage" = "Passwords must match"
        }
    };

    function register() {
        data.isRegistering = true;
        
        // Validate using cbValidation
        if (!validateOrFail()) {
            data.isRegistering = false;
            return;
        }
        
        // Register the user
        userService = getInstance("UserService");
        result = userService.createUser({
            "firstName" = data.firstName,
            "lastName" = data.lastName,
            "email" = data.email,
            "password" = data.password
        });
        
        if (result.success) {
            // Redirect to login page
            redirect("/login");
        } else {
            // Handle server-side errors
            data.isRegistering = false;
            return;
        }
        
        data.isRegistering = false;
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
<form wire:submit="register" class="registration-form">
    
    <div>
        <input type="text" wire:model="firstName" placeholder="First Name" required>
        <bx:if hasError("firstName")><span class="error">#getError("firstName")#</span></bx:if>
    </div>
    
    <div>
        <input type="text" wire:model="lastName" placeholder="Last Name" required>
        <bx:if hasError("lastName")><span class="error">#getError("lastName")#</span></bx:if>
    </div>
    
    <div>
        <input type="email" wire:model="email" placeholder="Email" required>
        <bx:if hasError("email")><span class="error">#getError("email")#</span></bx:if>
    </div>
    
    <div>
        <input type="password" wire:model="password" placeholder="Password" required>
        <bx:if hasError("password")><span class="error">#getError("password")#</span></bx:if>
    </div>
    
    <div>
        <input type="password" wire:model="confirmPassword" placeholder="Confirm Password" required>
        <bx:if hasError("confirmPassword")><span class="error">#getError("confirmPassword")#</span></bx:if>
    </div>
    
    <button type="submit">
        <bx:if isRegistering>Creating Account...<bx:else>Create Account</bx:if>
    </button>
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userRegistration.cfm -->
<cfoutput>
<form wire:submit="register" class="registration-form">
    
    <div>
        <input type="text" wire:model="firstName" placeholder="First Name" required>
        <cfif hasError("firstName")><span class="error">#getError("firstName")#</span></cfif>
    </div>
    
    <div>
        <input type="text" wire:model="lastName" placeholder="Last Name" required>
        <cfif hasError("lastName")><span class="error">#getError("lastName")#</span></cfif>
    </div>
    
    <div>
        <input type="email" wire:model="email" placeholder="Email" required>
        <cfif hasError("email")><span class="error">#getError("email")#</span></cfif>
    </div>
    
    <div>
        <input type="password" wire:model="password" placeholder="Password" required>
        <cfif hasError("password")><span class="error">#getError("password")#</span></cfif>
    </div>
    
    <div>
        <input type="password" wire:model="confirmPassword" placeholder="Confirm Password" required>
        <cfif hasError("confirmPassword")><span class="error">#getError("confirmPassword")#</span></cfif>
    </div>
    
    <button type="submit">
        <cfif isRegistering>Creating Account...<cfelse>Create Account</cfif>
    </button>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Best Practices

### Automatic Form Protection

CBWIRE automatically prevents double submissions by disabling form elements during processing. You don't need to manually add `disabled` attributes - just provide visual feedback about the current state:

{% tabs %}
{% tab title="BoxLang" %}
```html
<button type="submit">
    <bx:if isProcessing>Processing...<bx:else>Submit</bx:if>
</button>
```
{% endtab %}

{% tab title="CFML" %}
```html
<button type="submit">
    <cfif isProcessing>Processing...<cfelse>Submit</cfif>
</button>
```
{% endtab %}
{% endtabs %}

### Provide User Feedback

Show clear loading states and progress indicators:

{% tabs %}
{% tab title="BoxLang" %}
```html
<bx:output>
<form wire:submit="processOrder">
    <!-- Form fields -->
    
    <bx:if isProcessing>
        <div class="processing-indicator">
            <span>Processing your order...</span>
            <div class="progress-bar">
                <div class="progress" style="width: #processingProgress#%"></div>
            </div>
        </div>
    </bx:if>
    
    <button type="submit">
        <bx:if isProcessing>Processing Order...<bx:else>Place Order</bx:if>
    </button>
</form>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<cfoutput>
<form wire:submit="processOrder">
    <!-- Form fields -->
    
    <cfif isProcessing>
        <div class="processing-indicator">
            <span>Processing your order...</span>
            <div class="progress-bar">
                <div class="progress" style="width: #processingProgress#%"></div>
            </div>
        </div>
    </cfif>
    
    <button type="submit">
        <cfif isProcessing>Processing Order...<cfelse>Place Order</cfif>
    </button>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

### Handle Errors Gracefully

Use cbValidation for comprehensive error handling:

```javascript
function submitForm() {
    data.isSubmitting = true;
    
    // Use cbValidation for validation
    if (!validateOrFail()) {
        data.isSubmitting = false;
        return;
    }
    
    try {
        // Process form
        result = processFormData();
        
        if (result.success) {
            // Success handling
            redirect("/success");
        } else {
            // Handle server-side errors appropriately
            data.isSubmitting = false;
            return;
        }
    } catch (any e) {
        // Log the error and handle gracefully
        logError(e);
        data.isSubmitting = false;
    }
    
    data.isSubmitting = false;
}
```

{% hint style="info" %}
`wire:submit` automatically prevents the default form submission behavior, so you don't need to add `preventDefault()` calls in your JavaScript.
{% endhint %}

{% hint style="warning" %}
Always use CBWIRE's built-in cbValidation for form validation rather than manual validation. This provides consistent error handling and integrates seamlessly with the validation display methods.
{% endhint %}