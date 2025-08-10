---
description: Set your data properties with incoming query string values.
---

# Query String

It can sometimes be helpful to update the browser's query string when your component's state changes.

Let's say we are building a component to search articles.

```html
<!--- ./wires/SearchArticles.cfm --->
<cfscript>
    data = {
        "search": ""
    };
</cfscript>

<cfoutput>
    <div>
        <input wire:model="search" type="search" placeholder="Search articles...">
    </div>
</cfoutput>
```

We can automatically populate the search property above from the URL query string by adding a _queryString_ array to our component.

```
queryString = [ "search" ];
```

```html
<!-- ./wires/SearchArticles.cfm -->
<cfscript>
    queryString = [ "search" ];

    data = {
        "search": ""
    };
</cfscript>

<cfoutput>
    <div>
        <input wire:model="search" type="search" placeholder="Search articles...">
    </div>
</cfoutput>
```

Now when we access the page with 'search' included in the query string (_/search-articles?search=some+string_), the search property will have a default value of "some string".

In addition to this, when the user starts typing into our search field, the URL displayed in the browser will be instantly updated.
