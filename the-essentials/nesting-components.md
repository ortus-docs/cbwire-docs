# Nesting Components

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ContactUs.bx
class extends="cbwire.models.Component" {
    data = {
        "showForm": false
    };  
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ContactUs.cfc
component extends="cbwire.models.Component" {
    data = {
        "showForm": false
    };  
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- wires/contactus.bxm --->
<bx:output>
    <div>
        <bx:if showForm>
            <!--- Nested component --->
            #wire( name="ContactForm" )#
        </bx:if>    
        <button wire:click="$toggle( 'showForm' )">Toggle form</button>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- wires/contactus.cfm --->
<cfoutput>
    <div>
        <cfif showForm>
            <!--- Nested component --->
            #wire( name="ContactForm" )#
        </cfif>    
        <button wire:click="$toggle( 'showForm' )">Toggle form</button>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

You can also pass parameters.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ContactUs.bx
class extends="cbwire.models.Component" {
    data = {
        "showForm": false,
        "sendEmail": false,
        "validateForm": true
    };  
    
    function onMount( params ) {
        data.sendEmail = params.sendEmail;
        data.validateForm = params.validateForm;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ContactUs.cfc
component extends="cbwire.models.Component" {
    data = {
        "showForm": false,
        "sendEmail": false,
        "validateForm": true
    };  
    
    function onMount( params ) {
        data.sendEmail = params.sendEmail;
        data.validateForm = params.validateForm;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- wires/contactus.bxm --->
<bx:output>
    <div>
        <bx:if showForm>
            #wire(
                name="ContactForm"
                params={
                    sendEmail: true,
                    validateForm: true
                }
            )#
        </bx:if>
        <button wire:click="$toggle( 'showForm' )">Toggle form</button>    
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- wires/contactus.cfm --->
<cfoutput>
    <div>
        <cfif showForm>
            #wire(
                name="ContactForm"
                params={
                    sendEmail: true,
                    validateForm: true
                }
            )#
        </cfif>
        <button wire:click="$toggle( 'showForm' )">Toggle form</button>    
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}
