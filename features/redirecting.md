# Redirecting

Redirect users to different pages or external URLs from your component actions. CBWIRE provides the `redirect()` method with options for traditional page redirects or single-page navigation using `wire:navigate`.

## Basic Usage

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/UserDashboard.bx
class extends="cbwire.models.Component" {
    data = {
        "isLoggedIn": false
    };
    
    function logout() {
        // Perform logout logic
        data.isLoggedIn = false;
        
        // Redirect to login page
        redirect("/login");
    }
    
    function goToProfile() {
        // Navigate to profile using wire:navigate (SPA-style)
        redirect("/user/profile", true);
    }
    
    function visitExternal() {
        // Redirect to external URL
        redirect("https://example.com");
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/UserDashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "isLoggedIn" = false
    };
    
    function logout() {
        // Perform logout logic
        data.isLoggedIn = false;
        
        // Redirect to login page
        redirect("/login");
    }
    
    function goToProfile() {
        // Navigate to profile using wire:navigate (SPA-style)
        redirect("/user/profile", true);
    }
    
    function visitExternal() {
        // Redirect to external URL
        redirect("https://example.com");
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/userDashboard.bxm -->
<bx:output>
<div>
    <h1>User Dashboard</h1>
    
    <bx:if isLoggedIn>
        <button wire:click="logout">Logout</button>
        <button wire:click="goToProfile">View Profile</button>
    <bx:else>
        <p>Please log in to continue.</p>
    </bx:if>
    
    <button wire:click="visitExternal">Visit Example Site</button>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/userDashboard.cfm -->
<cfoutput>
<div>
    <h1>User Dashboard</h1>
    
    <cfif isLoggedIn>
        <button wire:click="logout">Logout</button>
        <button wire:click="goToProfile">View Profile</button>
    <cfelse>
        <p>Please log in to continue.</p>
    </cfif>
    
    <button wire:click="visitExternal">Visit Example Site</button>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Redirect Types

### Internal Pages

Redirect to pages within your application:

```javascript
redirect("/dashboard");
redirect("/users/123");
redirect("/admin/settings");
```

### External URLs

Redirect to external websites:

```javascript
redirect("https://google.com");
redirect("https://github.com/user/repo");
```

### Wire Navigate

Use `wire:navigate` for SPA-style navigation without full page reloads:

```javascript
redirect("/dashboard", true);
```

This creates a faster, single-page application experience by only updating the page content without refreshing the entire browser window.

{% hint style="info" %}
Wire navigate (`redirect("/path", true)`) only works for internal application URLs. External URLs always perform traditional redirects regardless of the second parameter.
{% endhint %}

{% hint style="warning" %}
Redirects immediately terminate action execution. Any code after a `redirect()` call will not execute.
{% endhint %}
