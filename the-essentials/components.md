# Components

Components are sections of your application that you want to be reactive to user input.

Components are the fundamental building blocks of CBWIRE applications. They encapsulate both the data and behavior needed to create interactive user interface elements that respond to user actions without requiring full page refreshes. Think of components as self-contained, reusable pieces of functionality that manage their own state and handle their own user interactions.

{% hint style="info" %}
**Components can be as big or small as you like.** For example, you may have a Signup form component that covers multiple steps or a simple button component that you reuse throughout your application.
{% endhint %}

Each CBWIRE component is a ColdBox component (CFC) that extends the base `cbwire.models.Component` class. This inheritance provides all the reactive functionality and lifecycle methods needed to create dynamic user interfaces. Components follow a clear structure and naming convention, making them easy to organize and maintain as your application grows.

Components are made up of [data properties](properties.md), [computed properties](computed-properties.md), [actions](actions.md), and a [template. ](templates.md) This separation of concerns keeps your code organized and maintainable while providing the flexibility to create complex interactive experiences.

When a component is rendered, CBWIRE automatically handles the initial setup, manages the component's lifecycle, and coordinates communication between the server and client. This abstraction allows you to focus on your application's business logic rather than the underlying mechanics of creating reactive interfaces.

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/Counter.bx
class extends="cbwire.models.Component" {

    // Data properties
    data = {
        "counter": 0 // default value
    };
    
    // Action
    function increment() {
        data.counter++;
    }
    
    // Helper method also available to template
    function isEven() {
        return data.counter % 2 == 0;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/Counter.cfc
component extends="cbwire.models.Component" {

    // Data properties
    data = {
        "counter": 0 // default value
    };
    
    // Action
    function increment() {
        data.counter++;
    }
    
    // Helper method also available to template
    function isEven() {
        return data.counter % 2 == 0;
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!--- ./wires/counter.bxm --->
<bx:output>
    <div>
        <h1>My Counter</h1>
        Counter: #counter#<br>
        Is Even: #isEven()#
        <button wire:click="increment">Increment</button>
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!--- ./wires/counter.cfm --->
<cfoutput>
    <div>
        <h1>My Counter</h1>
        Counter: #counter#<br>
        Is Even: #isEven()#
        <button wire:click="increment">Increment</button>
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
By default, components are placed in the **./wires** folder in the project root. You can change this location using the **wiresLocation** [configuration setting](../configuration.md).
{% endhint %}

## Rendering Components

You can render a component using the **wire()** method.&#x20;

```html
<!--- ./layouts/Main.bxm|cfm --->
<body>
    <div>
        #wire( name="ShowPosts" )#
    </div>
</body>
```

```html
<!--- ./views/posts/index.bxm|cfm --->
<h1>My Posts</h1>
#wire( name="ShowPosts" )#
```

{% hint style="info" %}
You can call **wire()** from within your ColdBox layouts, ColdBox views, and also from your component templates ( See Nesting Components ).
{% endhint %}

## External Components

You can render wires from folders and subfolders outside of the default **./wires** folder.

```html
<div>
    #wire( name="myFolder.MyComponent" )#
    #wire( name="myFolder.subFolder.MyComponent" )#
</div>
```

You can also reference components within another ColdBox module by using the @module syntax.

```html
<div>#wire( name="MyComponent@myModule" )#</div>
```

## Passing Parameters

You can pass data into a component as an additional argument using wire().

```html
<body>
    <div>
        #wire( name="ShowPost", params={ "post": post } )#
        #wire( "ShowPost", { "post": post } )#
    </div>
</body>
```

{% hint style="success" %}
By passing in parameters, you can create reusable UI components that are unique but similar in functionality. For example, you could make a button component that you reuse throughout your app.
{% endhint %}

## Using Parameters

Parameters are passed into your component's [**onMount()**](lifecycle-events.md#onmount) method. This is an optional method you can add.

```html
<!--- ./views/posts/index.bxm|cfm --->
<div>
    #wire( "ShowPost", { title: "Post 1" } )#
</div>
```

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// ./wires/ShowPost.bx
class extends="cbwire.models.Component" {
    data = {
        "title": ""
    };

    function onMount( params ) {
        data.title = params.title;
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// ./wires/ShowPost.cfc
component extends="cbwire.models.Component" {
    data = {
        "title": ""
    };
    function onMount( params ) {
        data.title = params.title;
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
CBWIRE only executes **onMount()** once when the component is initially rendered. It is not executed for subsequent update requests of the component.
{% endhint %}

## Auto Populating Data Properties

Properties you pass into your component as params will be automatically populated if **onMount()** is not defined and a matching [data property](properties.md) is found.&#x20;

```html
<div>
    #wire( name="ShowPost", params={ title: "Some title" } )#
</div>
```

{% hint style="warning" %}
Passed-in properties must have a data type of string, boolean, numeric, date, array, or struct.
{% endhint %}

## Nesting Components

{% hint style="info" %}
You can nest components as much as you need by simply calling **wire()** from within a  [template](templates.md) ( See [Nesting Components](components.md#nesting-components) ).
{% endhint %}

## CBWIREController

There may be areas of your application where you need to use **wire()** but don't have access to it. You can use the CBWIREController object to insert wires anywhere you need.

```javascript
// property injection
property name="CBWIREController" inject="CBWIREController@cbwire";

// using getInstance
CBWIREController = getInstance( "CBWIREController@cbwire" );

// Creating a wire from controller
CBWIREController.wire( "EditablePage", { contentKey: "someValue" } );
```
