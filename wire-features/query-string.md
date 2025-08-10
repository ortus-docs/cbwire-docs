# Query String

It can be useful at times to update the browser's query string when your [Wire's ](../essentials/creating-components.md)state changes.

## Example

Let's say you are building a [Wire](../essentials/creating-components.md) to search articles, and want the query string to reflect the current search value like so:

```
https://yourapp.com/search-articles?search=some+string
```

This way, when a user hits the back button or bookmarks the page, you can get the initial state out of the query string rather than resetting the [Wire](../essentials/creating-components.md) every time.

You can add a `queryString` variable to your [Wires](../essentials/creating-components.md) and CBWIRE will update the query string every time the property value changes and also update the property when the query string changes.

```javascript
component extends="cbwire.models.Component" {

    data = {
        "search": ""
    };
    
    queryString = [ "search" ];
}
```

```html
<div>
    <input wire:model="search" type="search" placeholder="Search articles...">
</div>
```
