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

## Using Keys with Nested Components

When nesting components, it's important to provide a **key** to help Livewire understand what's changed when components are updated. This is particularly crucial when you have dynamic nested components that may be added or removed from the DOM.

### Why Keys Matter

Livewire treats each nested component as an island, but sometimes you want to remove or delete child components entirely. The `key` parameter is what Livewire looks at to determine if a child component still exists or if it should be removed from the DOM.

Without keys, when a parent component refreshes, Livewire may not properly identify which nested components have changed, leading to unexpected behavior or components not being properly cleaned up.

### Basic Key Usage

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- wires/contactus.bxm --->
<bx:output>
    <div>
        <bx:if showForm>
            <!--- Using a key helps Livewire track this nested component --->
            #wire( name="ContactForm", key="contact-form-#generateUUID()#" )#
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
            <!--- Using a key helps Livewire track this nested component --->
            #wire( name="ContactForm", key="contact-form-#createUUID()#" )#
        </cfif>    
        <button wire:click="$toggle( 'showForm' )">Toggle form</button>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

### Dynamic Keys for Component Lists

When rendering multiple nested components dynamically, use unique keys to help Livewire track each component individually:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/PostList.bx
class extends="cbwire.models.Component" {
    data = {
        "posts": [
            { "id": 1, "title": "First Post" },
            { "id": 2, "title": "Second Post" },
            { "id": 3, "title": "Third Post" }
        ]
    };

    function removePost(postId) {
        data.posts = data.posts.filter(function(post) {
            return post.id != arguments.postId;
        });
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/PostList.cfc
component extends="cbwire.models.Component" {
    data = {
        "posts" = [
            { "id" = 1, "title" = "First Post" },
            { "id" = 2, "title" = "Second Post" },
            { "id" = 3, "title" = "Third Post" }
        ]
    };

    function removePost(postId) {
        data.posts = data.posts.filter(function(post) {
            return post.id != arguments.postId;
        });
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- wires/postlist.bxm --->
<bx:output>
    <div>
        <bx:loop array="#posts#" index="post">
            <!--- Each nested component gets a unique key based on the post ID --->
            #wire( 
                name="PostCard", 
                params={ "post": post },
                key="post-#post.id#"
            )#
        </bx:loop>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- wires/postlist.cfm --->
<cfoutput>
    <div>
        <cfloop array="#posts#" index="post">
            <!--- Each nested component gets a unique key based on the post ID --->
            #wire( 
                name="PostCard", 
                params={ "post": post },
                key="post-#post.id#"
            )#
        </cfloop>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

### Key Guidelines

- **Use unique identifiers**: Base keys on database IDs, UUIDs, or other unique values
- **Keep keys stable**: Don't use random values that change on each render
- **Be descriptive**: Use meaningful key names like `"user-#userID#"` or `"form-#formType#"`
- **Required for dynamic lists**: Always use keys when rendering nested components in loops
- **Essential for conditional rendering**: Use keys when components are conditionally displayed

{% hint style="info" %}
When a key changes or is removed, Livewire will destroy the old component and create a new one. This ensures proper cleanup of event listeners, timers, and other component state.
{% endhint %}

{% hint style="warning" %}
Keys must be unique across all nested components on the page, not just within a single parent component. Duplicate keys can cause rendering issues and unpredictable behavior.
{% endhint %}
