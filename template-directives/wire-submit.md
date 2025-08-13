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
    
    <button type="submit" <bx:if isSubmitting>disabled</bx:if>>
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
    
    <button type="submit" <cfif isSubmitting>disabled</cfif>>
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
- **Maintains State**: Keeps all your component data intact during the submission process
- **Provides Feedback**: Allows you to show loading states and disable buttons during processing

## Submit Modifiers

Customize form submission behavior with modifiers:

### Prevent Modifier

Use `wire:submit.prevent` to explicitly prevent the default form submission (though this is the default behavior):

```html
<form wire:submit.prevent="saveUser">
    <!-- Form fields -->
</form>
```


## Practical Examples

### User Registration Form

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
        "isRegistering": false,
        "errors": {}
    };

    function register() {
        data.isRegistering = true;
        data.errors = {};
        
        // Validate inputs
        if (!data.firstName.trim().len()) {
            data.errors.firstName = "First name is required";
        }
        
        if (!data.email.trim().len() || !isValid("email", data.email)) {
            data.errors.email = "Valid email is required";
        }
        
        if (data.password != data.confirmPassword) {
            data.errors.confirmPassword = "Passwords must match";
        }
        
        // If validation fails, stop here
        if (data.errors.keyArray().len()) {
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
            // Redirect or show success message
            relocate("login");
        } else {
            data.errors.general = result.message;
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
        "isRegistering" = false,
        "errors" = {}
    };

    function register() {
        data.isRegistering = true;
        data.errors = {};
        
        // Validate inputs
        if (!len(trim(data.firstName))) {
            data.errors.firstName = "First name is required";
        }
        
        if (!len(trim(data.email)) || !isValid("email", data.email)) {
            data.errors.email = "Valid email is required";
        }
        
        if (data.password != data.confirmPassword) {
            data.errors.confirmPassword = "Passwords must match";
        }
        
        // If validation fails, stop here
        if (structCount(data.errors)) {
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
            // Redirect or show success message
            relocate("login");
        } else {
            data.errors.general = result.message;
        }
        
        data.isRegistering = false;
    }
}
```
{% endtab %}
{% endtabs %}

```html
<form wire:submit="register" class="registration-form">
    <bx:if errors.keyExists("general")><div class="error">#errors.general#</div></bx:if>
    
    <div>
        <input type="text" wire:model="firstName" placeholder="First Name" required>
        <bx:if errors.keyExists("firstName")><span class="error">#errors.firstName#</span></bx:if>
    </div>
    
    <div>
        <input type="text" wire:model="lastName" placeholder="Last Name" required>
    </div>
    
    <div>
        <input type="email" wire:model="email" placeholder="Email" required>
        <bx:if errors.keyExists("email")><span class="error">#errors.email#</span></bx:if>
    </div>
    
    <div>
        <input type="password" wire:model="password" placeholder="Password" required>
    </div>
    
    <div>
        <input type="password" wire:model="confirmPassword" placeholder="Confirm Password" required>
        <bx:if errors.keyExists("confirmPassword")><span class="error">#errors.confirmPassword#</span></bx:if>
    </div>
    
    <button type="submit" <bx:if isRegistering>disabled</bx:if>>
        <bx:if isRegistering>Creating Account...<bx:else>Create Account</bx:if>
    </button>
</form>
```

### Product Search Form

```html
<form wire:submit="searchProducts" class="search-form">
    <div class="search-inputs">
        <input type="text" wire:model="searchTerm" placeholder="Search products...">
        <select wire:model="category">
            <option value="">All Categories</option>
            <bx:loop array="#categories#" index="cat">
                <option value="#cat.id#">#cat.name#</option>
            </bx:loop>
        </select>
    </div>
    
    <button type="submit">Search</button>
</form>

<bx:if searchResults.len()>
    <div class="search-results">
        <bx:loop array="#searchResults#" index="product">
            <div class="product-card">
                <h3>#product.name#</h3>
                <p>#product.description#</p>
                <span class="price">$#product.price#</span>
            </div>
        </bx:loop>
    </div>
</bx:if>
```


## Form Validation Integration

`wire:submit` works seamlessly with form validation:

```html
<form wire:submit="saveSettings">
    <div>
        <input type="text" wire:model="siteName" required>
        <bx:if errors.keyExists("siteName")>
            <span class="error">#errors.siteName#</span>
        </bx:if>
    </div>
    
    <div>
        <input type="email" wire:model="adminEmail" required>
        <bx:if errors.keyExists("adminEmail")>
            <span class="error">#errors.adminEmail#</span>
        </bx:if>
    </div>
    
    <button type="submit">Save Settings</button>
</form>
```

## Best Practices

### Prevent Double Submissions

Always disable the submit button during processing:

```html
<button type="submit" <bx:if isProcessing>disabled</bx:if>>
    <bx:if isProcessing>Processing...<bx:else>Submit</bx:if>
</button>
```

### Provide User Feedback

Show clear loading states and progress indicators:

```html
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
    
    <button type="submit" <bx:if isProcessing>disabled</bx:if>>
        Place Order
    </button>
</form>
```

### Handle Errors Gracefully

Display validation errors clearly and reset states appropriately:

```javascript
function submitForm() {
    data.isSubmitting = true;
    data.errors = {};
    
    try {
        // Process form
        result = processFormData();
        
        if (result.success) {
            // Success handling
        } else {
            data.errors = result.errors;
        }
    } catch (any e) {
        data.errors.general = "An unexpected error occurred. Please try again.";
    }
    
    data.isSubmitting = false;
}
```

{% hint style="info" %}
`wire:submit` automatically prevents the default form submission behavior, so you don't need to add `preventDefault()` calls in your JavaScript.
{% endhint %}

{% hint style="warning" %}
Remember to validate form data both on the client side for user experience and on the server side for security. Never trust client-side validation alone.
{% endhint %}