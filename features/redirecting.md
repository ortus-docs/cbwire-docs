# Redirecting

You can redirect users in your actions using redirect().

{% tabs %}
{% tab title="BoxLang" %}
```javascript
class extends="cbwire.models.Component" {
    
    function redirectUser() {
        // Redirect to a URI
        redirect( "/some-uri" );
        // Redirect to a URL
        redirect( "https://www.google.com" );
        // Redirect using wire:navigate
        redirect( "/some-uri", true );
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
component extends="cbwire.models.Component" {
    
    function redirectUser() {
        // Redirect to a URI
        redirect( "/some-uri" );
        // Redirect to a URL
        redirect( "https://www.google.com" );
        // Redirect using wire:navigate
        redirect( "/some-uri", true );
    }
}
```
{% endtab %}
{% endtabs %}

```html
<div>
    <button wire:click="redirectUser">Redirect Me</button>
</div>
```
