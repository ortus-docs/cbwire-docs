# wire:init

You can run an [action](../the-essentials/actions.md) once your component is rendered in the browser using **wire:init**. This can be helpful when you don't want to hold up loading the entire page but want to load some data immediately after the page loads.

{% hint style="info" %}
The need for wire:init has largely been replaced by CBWIRE's [Lazy Loading](../features/lazy-loading.md), but still exists and can be used if you prefer.
{% endhint %}

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/MyComponent.bx
class extends="cbwire.models.Component" {
    data = {
        "loaded": false    
    };
    function loadData() {
        sleep( 2000 ); // pretend this takes a while
        data.loaded = true;    
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/MyComponent.cfc
component extends="cbwire.models.Component" {
    data = {
        "loaded": false    
    };
    function loadData() {
        sleep( 2000 ); // pretend this takes a while
        data.loaded = true;    
    }
}
```
{% endtab %}
{% endtabs %}

```html
<!--- ./wires/mycomponent.cfm --->
<div wire:init="loadData">
    <cfif loaded>
        <div>Data is now loaded.</div>
    <cfelse>
        <div>Loading...</div>
    </cfif>
</div>
```

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/mycomponent.bxm --->
<div wire:init="loadData">
    <bx:if loaded>
        <div>Data is now loaded.</div>
    <bx:else>
        <div>Loading...</div>
    </bx:if>
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/mycomponent.cfm --->
<div wire:init="loadData">
    <cfif loaded>
        <div>Data is now loaded.</div>
    <cfelse>
        <div>Loading...</div>
    </cfif>
</div>
```
{% endtab %}
{% endtabs %}

