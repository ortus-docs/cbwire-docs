---
description: Sections or areas of your app that are reactive to user input.
---

# Components

Components are sections or areas of your site that you want to be reactive to user input. **They can be as big or small as you like**. For example, you may have a Signup form component that covers multiple steps or a button component that you reuse throughout your app.

Components are generally made up of [Data Properties](properties.md), [Computed Properties](computed-properties.md), [Actions](actions.md), and an HTML template.&#x20;

```html
<cfscript>
    // Data properties
    data = {
        "property1": "defaultValue",
        "property2": "defaultValue"
    };
    
    // Computed properties
    computed = {
        "computedProperty" : function() {
            // return something
        }    
    };
    
    // Actions
    function action1() {
        data.property1 = "another value";
    }
</cfscript>

<!-- HTML Template -->
<cfoutput>
    <div>
        <h1>My Component</h1>
    </div>
</cfoutput>
```

{% hint style="info" %}
Increase the performance of component rendering in your production environments with the 'cacheSingleFileComponents' [configuration](../configuration.md) setting.
{% endhint %}

{% hint style="info" %}
**Your template must have a single outer element for CBWIRE to bind to.** Our example above uses a **\<div>** element but can use any outer element. Adding more than one outer element will lead to DOM diffing issues and unexpected behavior.
{% endhint %}

{% hint style="info" %}
By default, components should be placed in a **./wires** folder in the project root.
{% endhint %}

{% hint style="success" %}
Within your templates, you can access your [Data Properties](properties.md), [Computed Properties](computed-properties.md), and [Actions](actions.md), and utilize any of the powerful template features listed under Template Features in the left-hand menu. Details on how to use these
{% endhint %}

## Separating Component Definition and Template

You can place the above code in separate files to separate your component and template definitions. **CFC components must extend 'cbwire.models.Component'**.

**./wires/MyComponent.cfc**

```javascript
component extends="cbwire.models.Component" {
    // Data properties
    data = {
        "property1": "defaultValue",
        "property2": "defaultValue"
    };
    
    // Computed properties
    computed = {
        "computedProperty" : function() {
            // return something
        }    
    };
    
    // Actions
    function action1() {
        data.property1 = "another value";
    }
}
```

**./wires/MyComponent.cfm**

```html
<!--- Template --->
<cfoutput>
    <div>
        <h1>My Component</h1>
    </div>
</cfoutput>
```

{% hint style="info" %}
Separating your template and component is how previous versions of CBWIRE worked. We recommend using the less verbose option of including both definitions in a single .CFM file for smaller UI components, and creating separate files for more complex UI components.
{% endhint %}

## Rendering Components

You can render a CBWIRE component using the **wire()** method.&#x20;

```html
<!--- ./layouts/Main.cfm --->
<body>
    <div>
        #wire( "ShowPosts" )#
    </div>
</body>
```

```html
<!--- ./views/posts/index.cfm --->
<cfoutput>
    <h1>My Posts</h1>
    #wire( "ShowPosts" )#
</cfoutput>
```

{% hint style="info" %}
You can call **wire()** from within your ColdBox layouts, ColdBox views, and also from your component templates ( nested components ).
{% endhint %}

## External Components

You can render wires from folders and subfolders outside of the default ./wires folder.

```html
<cfoutput>
    <div>
        #wire( "myFolder.MyComponent" )#
        #wire( "myFolder.subFolder.MyComponent" )#
    </div>
</cfoutput>
```

You can also reference components within another ColdBox module by using the @module syntax.

```html
<cfoutput>
    <div>#wire( "MyComponent@myModule" )#</div>
</cfoutput>
```

## Passing Parameters

You can pass data into a component as an additional argument using **wire()**.

```html
<body>
    <div>
        #wire( "ShowPost", { "post": post } )#
    </div>
</body>
```

{% hint style="success" %}
By passing in parameters, you can create reusable UI components that are unique but similar in functionality. For example, you could create a Button component that you use throughout your app.
{% endhint %}

## Using Parameters

Parameters are passed into your component's **onMount()** method. This is an optional method you can add.

```xml
<!--- ./views/posts/index.cfm --->
#wire( "ShowPost", { post: currentPost } )#

<!--- ./wires/ShowPost.cfm --->
<cfscript>
    data = {
        "title": ""
    };

    function onMount( params, event, rc, prc ) {
        data.title = params.post.getTitle();
    }
}
</cfscript>

<cfoutput>
    <div>Title: #title#</div>
</cfoutput>
```

{% hint style="info" %}
CBWIRE only executes **onMount()** once when the component is initially rendered. It is not executed for subsequent XHR requests of the component.
{% endhint %}

## Auto Population of Data Properties

As of CBWIRE 3.2, properties that you pass into your component will be automatically populated if onMount() IS NOT defined and a matching data property is found.&#x20;

{% hint style="info" %}
Passed-in properties must have a data type of string, boolean, numeric, date, array, or struct.
{% endhint %}

```html
<!--- ./views/posts/index.cfm --->
#wire( "ShowPost", { title: "Some title" } )#

<!--- ./wires/ShowPost.cfm --->
<cfscript>
    data = {
        "title": ""
    };
}
</cfscript>

<cfoutput>
    <!--- outputs 'Title: Some title --->
    <div>Title: #title#</div>
</cfoutput>
```
