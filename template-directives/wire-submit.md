# wire:submit

The `wire:submit` directive provides a seamless way to handle form submissions in your CBWIRE components. Instead of dealing with traditional form processing and page refreshes, `wire:submit` intercepts form submissions and calls your component methods directly, creating smooth, dynamic form experiences.

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

## Basic Usage

This simple form demonstrates `wire:submit` with form handling, validation, error display, and loading states:

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
    
    constraints = {
        "name": {
            "required": true,
            "requiredMessage": "Name is required"
        },
        "email": {
            "required": true,
            "type": "email",
            "requiredMessage": "Email is required"
        }
    };

    function submitContact() {
        data.isSubmitting = true;
        
        // Validate using cbValidation
        if (!validateOrFail()) {
            data.isSubmitting = false;
            return;
        }
        
        // Process the form
        // (simulate processing time)
        sleep(1000);
        
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
    
    constraints = {
        "name" = {
            "required" = true,
            "requiredMessage" = "Name is required"
        },
        "email" = {
            "required" = true,
            "type" = "email",
            "requiredMessage" = "Email is required"
        }
    };

    function submitContact() {
        data.isSubmitting = true;
        
        // Validate using cbValidation
        if (!validateOrFail()) {
            data.isSubmitting = false;
            return;
        }
        
        // Process the form
        // (simulate processing time)
        sleep(1000);
        
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
        <input type="text" wire:model="name" placeholder="Your Name" required>
        <bx:if hasError("name")><span class="error">#getError("name")#</span></bx:if>
    </div>
    
    <div>
        <input type="email" wire:model="email" placeholder="Email" required>
        <bx:if hasError("email")><span class="error">#getError("email")#</span></bx:if>
    </div>
    
    <div>
        <textarea wire:model="message" placeholder="Your message" rows="4"></textarea>
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
        <input type="text" wire:model="name" placeholder="Your Name" required>
        <cfif hasError("name")><span class="error">#getError("name")#</span></cfif>
    </div>
    
    <div>
        <input type="email" wire:model="email" placeholder="Email" required>
        <cfif hasError("email")><span class="error">#getError("email")#</span></cfif>
    </div>
    
    <div>
        <textarea wire:model="message" placeholder="Your message" rows="4"></textarea>
    </div>
    
    <button type="submit">
        <cfif isSubmitting>Sending...<cfelse>Send Message</cfif>
    </button>
</form>
</cfoutput>
```
{% endtab %}
{% endtabs %}

