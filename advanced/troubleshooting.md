# Troubleshooting

## Overview

As you use CBWIRE, you are likely to run into issues from time to time. The most common issues are rendering issues. Here, we try to address the most common problems and show how to solve them.

## CBWIRE

### Lazy-loaded components with placeholders don't render

Consider the following widget example.

<pre class="language-javascript"><code class="lang-javascript"><strong>// ./wires/Widget.cfc
</strong>component extends="cbwire.models.Component" {
    function placeholder() {
        return "&#x3C;section>put spinner here...&#x3C;/section>";
    }
}
</code></pre>

```html
<!--- ./wires/widget.cfm --->
<cfoutput>
    <div>
        <h1>My Widget</h1>
    </div>
</cfoutput>
```

Let's include our widget somewhere on our site and add lazy loading.

```html
<cfoutput>#wire( name="Widget", lazy=true )#</cfoutput>
```

What should happen on page load is our placeholder is shown first, and then our widget renders to the page. Instead, we get a weird error like this.

**`Snapshot missing on Livewire component with id K8SDFLSDF902KSDFLASKJFASDFLJ.`**

The Livewire JavaScript error isn't super helpful in identifying the problem because **the real issue is a mismatch between your widget's outer element and its placeholder's outer element**.

Notice our widget's template above has an outer **\<div>** but our placeholder has an outer **\<section>** element. This will not work. **Your placeholder and your corresponding template must have the same outer element. Otherwise, Livewire's DOM diffing engine gets confused.**

The fix would be to make them both have outer \<div> tags.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Widget.bx
class extends="cbwire.models.Component" {
    function placeholder() {
        return "<div>put spinner here...</div>";
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Widget.cfc
component extends="cbwire.models.Component" {
    function placeholder() {
        return "<div>put spinner here...</div>";
    }
}
```
{% endtab %}
{% endtabs %}

```html
<!--- ./wires/widget.bxm|cfm --->
<div>
    <h1>My Widget</h1>
</div>
```

### Fix Rendering Conditionals with \<!---\[if BLOCK]>

When an [Action](../the-essentials/actions.md) is executed on your components, the component is re-rendered, and then it's Livewire's job to figure out how to update the DOM. For the most part, the DOM updates are completed without issue. However, you may run into problems with conditional areas ( if-else statements ) in your [Templat](../the-essentials/templates.md)[es](../the-essentials/templates.md).

Consider the following:

{% tabs %}
{% tab title="BoxLang" %}
```html
<div>
    <h1>My Form</h1>
    <bx:if errorList.len()>
        <div class="alert alert-danger" role="alert">
            <p>An error occurred.</p>
            <ul>
                <bx:loop array="#errorList#" index="error">
                    <li>#error#</li>
                </bx:loop>
            </ul>
        </div>
    </bx:if>
    ....
</div>
```
{% endtab %}

{% tab title="CFML" %}
<pre class="language-html"><code class="lang-html"><strong>&#x3C;div>
</strong>    &#x3C;h1>My Form&#x3C;/h1>
    &#x3C;cfif ArrayLen( errorList )>
        &#x3C;div class="alert alert-danger" role="alert">
            &#x3C;p>An error occured.&#x3C;/p>
            &#x3C;ul>
                &#x3C;cfloop array="#errorList#" index="error">
                    &#x3C;li>#error#&#x3C;/li>
                &#x3C;/cfloop>
            &#x3C;/ul>
        &#x3C;/div>
    &#x3C;/cfif>
    ....
&#x3C;/div>
</code></pre>
{% endtab %}
{% endtabs %}

Notice we have an alert that is displayed when there are errors in our `errorList`variable.&#x20;

Depending on the additional sections within your page and how deeply nested each section is, Livewire might have difficulty detecting the alert has been added on re-rendering and that a DOM update is required.&#x20;

We can let Livewire know there is a conditional section by adding an if-BLOCK comment.

```html
<!--[if BLOCK]><![endif]-->
....
<!--[if ENDBLOCK]><![endif]-->
```

{% tabs %}
{% tab title="BoxLang" %}
```html
<div>
    <h1>My Form</h1>
    <!--[if BLOCK]><![endif]-->
    <bx:if errorList.len()>
        <div class="alert alert-danger" role="alert">
            <p>An error occurred.</p>
            <ul>
                <bx:loop array="#errorList#" index="error">
                    <li>#error#</li>
                </bx:loop>
            </ul>
        </div>
    </bx:if>
    <!--[if ENDBLOCK]><![endif]-->
    ....
</div>
```
{% endtab %}

{% tab title="CFML" %}
```html
<div>
    <h1>My Form</h1>
    <!--[if BLOCK]><![endif]-->
    <cfif ArrayLen( errorList )>
        <div class="alert alert-danger" role="alert">
            <p>An error occured.</p>
            <ul>
                <cfloop array="#errorList#" index="error">
                    <li>#error#</li>
                </cfloop>
            </ul>
        </div>
    </cfif>
    <!--[if ENDBLOCK]><![endif]-->
    ....
</div>
```
{% endtab %}
{% endtabs %}

## Alpine.js

### Nothing is working

While Alpine is lightweight and simple to use, it's easy to get tripped up when getting started.

See if you can spot what is wrong with this code.

```html
<div
    x-data="{
        name: "CBWIRE"
    }">
    <div>Name: <span x-text="name"></span></div>
</div>
```

{% hint style="danger" %}
The code above will result in a JavaScript error.&#x20;
{% endhint %}

**The problem is our use of double quotes for our name property.** Because our **x-data** block is surrounded by double quotes, using double quotes inside the block isn't proper syntax.

Change to using single quotes instead, and all is well again.

```html
<div
    x-data="{
        name: 'CBWIRE'
    }">
    <div>Name: <span x-text="name"></span></div>
</div>
```

{% hint style="info" %}
We recommend always using single quotes inside your x-data block.
{% endhint %}

### Unable to find component error

You can run into rendering issues when using Alpine's **\<template>** tag with an **x-if** if you are also including a component within the tag using wire().

For example:

```html
<div x-data="{
    loading: false,
    async init() {
        this.loading = true
        await $wire.someAction()
        this.loading = false
    }
}">
    <template x-if="loading">
        #wire( "Spinner" )#
    </template>
</div>
```

As you toggle the loading value from true to false, you may see a JavaScript error similar to this. The component ID will be different

**`Unable to find component K8SDFLSDF902KSDFLASKJFASDFLJ.`**

The issue above is that Alpine.js' x-if directive completely removes the contents inside from the DOM and recreates them when true. Livewire.js' DOM diffing engine is expecting the spinner to be there, and it can't find it, hence the error.

Luckily, the simple fix is to change your **\<template x-if>** to something like a **\<div>** and use **x-show** instead.

```html
<div x-show="loading">
    #wire( "Spinner" )
</div>
```

You'll still achieve the desired result and Livewire.js can track the spinner accurately.
