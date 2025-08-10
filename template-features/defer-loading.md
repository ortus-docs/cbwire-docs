---
description: Call an action immediately once your Wire is first rendered.
---

# Defer Loading

You can use the `wire:init` [Directive](directives.md) to execute an action as soon as your component is initially rendered.

{% hint style="success" %}
This can be helpful in cases where you don't want to hold up the entire page load, but want to load some data immediately after the page load.
{% endhint %}

```html
<div wire:init="loadTasks">
    <ul>
        <cfloop array="#args.tasks#" index="task">
            <li>#task#</li>
        </cfloop>
    </ul>
</div>
```

```javascript
component extends="cbwire.models.Component" {

    property name="taskService" inject="TaskService@myapp";
    
    data = {
        "tasks": []
    };
    
    function loadTasks(){
        data.tasks = taskService.asMemento().get();
    }
    
}
```
