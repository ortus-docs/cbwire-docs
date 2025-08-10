---
description: The HTML that makes up your components.
---

# Templates

Templates are the HTML section of your [Components](components.md) and consist of any valid CFML tags. This includes \<cfif>, \<cfloop>, etc.

```html
<cfscript>
    // Component stuff goes here
    function doSomething() {}
</cfscript>

<cfoutput>
    <!-- HTML template goes here -->
    <div>
        <button wire:click="doSomething">Click here</button>
        <!-- Any valid CFML here -->
        <cfif datePart( 'h', now() ) lt 12>
            <p>Good morning!</p>
        <cfelse>
            <p>Good afternoon!</p>
        </cfif>
    </div>
</cfoutput>
```

## Outer Element

Your templates must have a single outer element for CBWIRE to properly bind to your component and update the DOM. This can be any valid HTML element. Below we are using a DIV element.

```html
<cfoutput>
    <!-- GOOD: single outer element -->
    <div>
        <p>My awesome component</p>
    </div>
</cfoutput>

<cfoutput>
    <!-- BAD: 2 outer elements -->
    <div>
        My awesome component    
    </div>
    <div>
        <p>This won't work.</p>
    </div>
</cfoutput>
```

## Accessing Properties

You can access your [Data Properties](templates.md#data-properties) from your template by calling the property name.

```html
<cfscript>
    data = {
        "greeting": "Hello from CBWIRE"
    };
</cfscript>

<cfoutput>
    <div>
        Greeting: #greeting#
    </div>
</cfoutput>
```

You can access your [Computed Properties](computed-properties.md) from your template by calling the property name as a method.

```html
<cfscript>
    computed = {
        "greeting": function() { 
            return "Hello from CBWIRE" 
        }
    };
</cfscript>

<cfoutput>
    <div>
        Greeting: #greeting()#
    </div>
</cfoutput>
```

## Helper Methods

You can access any global helper methods you have defined in your ColdBox application, along with any modules you have installed.

For example, if you've installed the cbi8n module ( an internalization module for ColdBox ), you can access its global helper methods in your template, such as the `$r()` method for displaying text in various languages.

```html
<cfoutput>
    <div>
        #$r( "greeting" )#
    </div>
</cfoutput>
```

## Nesting Components

You can nest components as much as you need by simply calling **wire()** from within a component's template.

```html
<!--- ./wires/ShowPosts.cfm --->
<cfscript>
    // Data properties, actions go here.
</cfscript>

<cfoutput>
    <div>
        <h1>Posts</h1>
        #wire( "AddPostForm" )#
        ...
    </div>
</cfoutput>
```

You can also conditionally render nested components.

```html
<cfscript>
    data = {
        "showForm" : false
    };
</cfscript>

<cfoutput>
    <div>
        <cfif showForm>
            #wire( "MyForm" )#
        </cfif>    
        <button wire:click="$toggle( 'showForm' )">Toggle form</button>
    </div>
</cfoutput>
```

