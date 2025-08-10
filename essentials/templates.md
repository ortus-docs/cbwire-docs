---
description: Your Wire's HTML. Simple as that. Honestly.
---

# Templates

{% hint style="warning" %}
[Wires](creating-components.md) require a template that contains the HTML to be rendered.&#x20;
{% endhint %}

{% hint style="info" %}
By default, Templates exist within your project under `./views/wires/[templateName].cfm`. You can override the default template location with the configuration setting `templatesLocation.` _See_ [_Configuration_](configuration.md)
{% endhint %}

## Implicit Path

If you haven't specified your template file name inside your Wire using **template**, a template will be implicitly loaded based on the Wire's file name.

```javascript
component extends="cbwire.models.Component" {
    // No template defined.
}
```

For the example above, the template `./views/wires/tasklist.cfm` will be rendered.

{% hint style="info" %}
File names should be lowercase when using Implicit Lookups. This is to avoid issues on case-sensitive file systems.
{% endhint %}

## Explicit Path

You can specify the path to your Template using `template`.&#x20;

```javascript
component extends="cbwire.models.Component" {
    template = "wires/tasklist"; // This will look for ./views/wires/tasklist.cfm

    // template = "path/to/tasklist"; <--- Can use folders
    
    // template = "tasklist.cfm"; <--- Can use .cfm if you like
}
```

{% hint style="warning" %}
In CBWIRE 1.x, you would define the template path using `view` but this has been deprecated and `template` should be used going forward.
{% endhint %}

## onRender

Instead of creating a .cfm template, you can define an onRender() method on your [Wire](creating-components.md).&#x20;

See the [Wire Lifecycle](lifecycle-events.md#onrender) page for additional details.

## Prevent Rendering

Within your [Actions](actions.md), you can call the method `noRender()` to prevent your template from re-rendering. This can reduce the returned payload size for actions that do not change the UI.&#x20;

```javascript
component extends="cbwire.model.Component"{
    // Action
    function updateSession(){
        // prevent UI updating
        noRender();
    }
}
```

## Outer Element

Be sure your template includes a single outer element such as`<div>`.&#x20;

```xml
<!--- GOOD --->
<cfoutput>
    <div>
        <button wire:click="addTask">Add Task</button>
    </div>
</cfoutput>

<!--- BAD --->
<cfoutput>
    <div>
        <button wire:click="addTask">Add Task</button>
    </div>
    <div>
        <h1>Tasks</h1>
    </div>
</cfoutput>
```

{% hint style="warning" %}
If an outer element is not detected, an **OuterElementNotFoundexception** is thrown.
{% endhint %}

{% hint style="success" %}
You can use something other than \<div>\</div>.
{% endhint %}
