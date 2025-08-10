---
description: You can listen for livewire:load and place any JavaScript there.
---

# Inline Scripts

We recommend you use [AlpineJS](https://alpinejs.dev/) for most of your JavaScript needs, but you can use `<script>` tags directly inside your [Templates](../essentials/templates.md).

```html
<div>
    <!--- Your component's template --->
    <script>
        document.addEventListener('livewire:load', function () {
            // Your JS here.
        })
    </script>
</div>
```

{% hint style="warning" %}
Your scripts will be run only once upon the first render of the component. If you need to run a JavaScript function later, you can emit the [Event](../essentials/events.md) from the component and listen to it in JavaScript.
{% endhint %}
