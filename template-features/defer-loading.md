---
description: Improve user's perceived load times by using deferred loading.
---

# Defer Loading

CBWIRE allows you to call an action immediately once your component is rendered.

You can do this by adding the _wire:init_ directive to your component's outer element.

```html
<cfoutput>
<div wire:init="loadTasks">
    <ul>
        <cfloop array="#args.tasks#" index="task">
            <li>#task#</li>
        </cfloop>
    </ul>
</div>
</cfoutput>

<cfscript>
    data = {
        "tasks": []
    };
    
    function loadTasks() {
        data.tasks = getInstance( "TaskService" ).asMemento().get()
    }
</cfscript>
```

{% hint style="info" %}
This can be helpful in cases where you don't want to hold up the page from fully loading but want to load some data immediately after the page load.
{% endhint %}
