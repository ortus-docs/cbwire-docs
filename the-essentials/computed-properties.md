# Computed Properties

Computed Properties are dynamic properties that are cached per component rendering.

## Differences

Computed Properties are similar to [Data Properties](properties.md) with some key differences:

* They are declared as functions within your component with a computed attribute added.
* They are cached.
* They can return any CFML data type, not just values that can be parsed by JavaScript like [Data Properties](properties.md).

{% hint style="info" %}
Computed properties are meant to return values and not change state such as modifying [Data Properties](properties.md). If you need to update a data property, use [Actions](actions.md) instead.
{% endhint %}

## Defining Computed Properties

You can define Computed Properties on your components as functions with the computed attribute added.&#x20;

{% tabs %}
{% tab title="BoxLang" %}
```javascript
class extends="cbwire.models.Component" {   

    // Computed property
    function isEven() computed {
        return data.counter % 2 == 0;
    }

}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
component extends="cbwire.models.Component" {   

    // Computed property
    function isEven() computed {
        return data.counter % 2 == 0;
    }

}
```
{% endtab %}
{% endtabs %}

## Templates

You can access Computed Properties in your component template using propertyName().

{% tabs %}
{% tab title="BoxLang" %}
```html
<bx:output>
    <div>
        Is Even: #isEven()#
    </div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<cfoutput>
    <div>
        Is Even: #isEven()#
    </div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

## Actions

You can also access Computed Properties from within your [Actions](computed-properties.md#actions).

```javascript
// Computed property
function isEven() computed {
    return data.counter % 2 == 0;
}

// Action
function increment() {
    if ( isEven() ) {
        data.counter += 2;
    } else {
        data.counter += 1;
    }
}
```

## Caching

Computed Properties cache their results for the lifetime of the request. It will only execute once if you reference your Computed Property three times in your component template or from within a component action.

```javascript
// Computed properties
function getUUID() computed {
    return createUUID();
}
```

```html
<div>
    <p>#getUUID()#</p>
    <p>#getUUID()#</p> <!-- Outputs the same ID because of caching -->
</div>
```

You can prevent caching on a computed property by passing a false argument when invoking it.

```html
<div>
    <p>#getUUID()#</p>
    <p>#getUUID( false )#</p> <!-- Does not have the same ID because caching disabled -->
</div>
```
