# Contact Form

Below is an example of a simple contact form with name, email, phone, and message fields. The fields use CBWIRE's built-in [Validation](../wire-features/validation.md) features.

For the validations to work, you must have _cbValidation_ installed. You can install it using CommandBox like so:

```
box install cbvalidation
```

## wires/contactForm.cfm

```html
<cfscript>
    // Constraints
    constraints = {
        "name": {
            "required": true
        },
        "email": {
            "required": true,
            "type": "email"
        },
        "phone": {
            "required": true,
            "type": "telephone"
        },
        "message": {
            "required": true,
            "size": "10..500"
        }
    };

    // Data Properties
    data = {
        "name": "",
        "email": "",
        "phone": "",
        "message": "",
        "success": false
    };

    // Actions
    function submitForm() {
        data.success = true;
    }
</cfscript>

<!--- Template --->
<cfoutput>
    <form wire:submit.prevent="submitForm" id="contactForm">
        <div class="form-floating">
            <input wire:model.lazy="name" class="form-control" id="name" type="text" placeholder="Enter your name..." data-sb-validations="required" />
            <label for="name">Name</label>
            <cfif validation.hasErrors( "name" )>
                <div class="text-danger">
                    #validation.getAllErrors( "name" ).first()#
                </div>
            </cfif>
        </div>
        <div class="form-floating">
            <input wire:model.lazy="email" class="form-control" id="email" type="email" placeholder="Enter your email..." data-sb-validations="required,email" />
            <label for="email">Email address</label>
            <cfif validation.hasErrors( "email" )>
                <div class="text-danger">
                    #validation.getAllErrors( "email" ).first()#
                </div>
            </cfif>
        </div>
        <div class="form-floating">
            <input wire:model.lazy="phone" class="form-control" id="phone" type="tel" placeholder="Enter your phone number..." data-sb-validations="required" />
            <label for="phone">Phone Number</label>
            <cfif validation.hasErrors( "phone" )>
                <div class="text-danger">
                    #validation.getAllErrors( "phone" ).first()#
                </div>
            </cfif>
        </div>
        <div class="form-floating">
            <textarea wire:model.lazy="message" class="form-control" id="message" placeholder="Enter your message here..." style="height: 12rem" data-sb-validations="required"></textarea>
            <label for="message">Message</label>
            <cfif validation.hasErrors( "message" )>
                <div class="text-danger">
                    #validation.getAllErrors( "message" ).first()#
                </div>
            </cfif>
        </div>
        <br />
        <!-- Submit success message-->
        <cfif success>
            <div id="submitSuccessMessage">
                <div class="text-center mb-3">
                    <div class="fw-bolder">Form submission successful!</div>
                </div>
            </div>
        </cfif>
        <!-- Submit Button-->
        <button
            class="btn btn-primary text-uppercase <cfif validation.hasErrors()>disabled</cfif>"
            id="submitButton"
            type="submit">Send</button>
    </form>
</cfoutput>
```
