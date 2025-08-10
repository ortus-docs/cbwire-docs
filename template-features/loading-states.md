---
description: >-
  Show your users loading spinners, modals, and more as they wait for requests
  to complete.
---

# Loading States

Actions may involve a long-running process (such as completing a cart checkout) and HTML may not re-render instantly. You can help your users during the wait using Loading States. Loading States allow you to show/hide elements, add/remove classes, or toggle HTML attributes until the server responds.

{% hint style="success" %}
Loading States can make your apps feel much more responsive and user-friendly.
{% endhint %}

## Toggling Elements

Let's look at an action that takes too long.

```javascript
<cfscript>
    function addTask(){
        sleep( 5000 ); // Optimize this code yo!
    }
</cfscript>
```

We can annotate our \<div> with _wire:loading_ to display _Adding Task_ while the checkout action is running.

```xml
<cfoutput>
    <div>
        <button wire:click="addTask">Add Task</button>
    
        <div wire:loading>
            Adding Task...
        </div>
    </div>
</cfoutput>

<cfscript>
    function addTask(){
        sleep( 5000 ); // Optimize this code yo!
    }
</cfscript>
```

After the action completes, the _Adding Task..._ output will disappear. :wave:&#x20;

## Toggling Attributes

```xml
<cfoutput>
    <div>
        <button wire:click="addTask" wire:loading.attr="disabled">
            Add Task
        </button>
    </div>
</cfoutput>
```

## Targeting Specific Actions

You can target a specific action so that the element is only visible if a specific action is loading using _wire:target_.

```html
<cfoutput>
   <div>
       <button wire:click="addTask">Add Task</button>
    
       <div wire:loading wire:target="addTask">
          Adding task...
       </div>
   </div>
</cfoutput>
```

## Delaying

If you want to avoid flickering because loading is very fast, you can add a _.delay_ modifier, and it will only show up if loading takes longer than 200ms.

```html
<div wire:loading.delay>...</div>
```

If you wish, you can customize the delay duration with the following modifiers:

```xml
<div wire:loading.delay.shortest>...</div> <!-- 50ms -->
<div wire:loading.delay.shorter>...</div>  <!-- 100ms -->
<div wire:loading.delay.short>...</div>    <!-- 150ms -->
<div wire:loading.delay>...</div>          <!-- 200ms -->
<div wire:loading.delay.long>...</div>     <!-- 300ms -->
<div wire:loading.delay.longer>...</div>   <!-- 500ms -->
<div wire:loading.delay.longest>...</div>  <!-- 1000ms -->
```

## Display Property

Loading State elements are set with a CSS property of _display: inline-block;_ by default. You can override this behavior by using various directive modifiers.

```xml
<div wire:loading.flex>...</div>
<div wire:loading.grid>...</div>
<div wire:loading.inline>...</div>
<div wire:loading.table>...</div>
```

## Hiding

If you want to display an element except during a loading state, you can apply the _wire:loading.remove_ directive.

<pre class="language-xml"><code class="lang-xml">&#x3C;cfoutput>
<strong>    &#x3C;div>
</strong>        &#x3C;button wire:click="addTask">Add Task&#x3C;/button>
    
        &#x3C;div wire:loading.remove>
            Click 'Add Task'
        &#x3C;/div>
    &#x3C;/div>
&#x3C;/cfoutput>
</code></pre>
