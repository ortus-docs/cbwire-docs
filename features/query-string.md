# Query String

Synchronize component data properties with URL query parameters to create shareable, bookmarkable URLs that reflect your component's state. When users modify data properties, the URL automatically updates, and when they visit URLs with query parameters, your component initializes with those values.

## Basic Usage

Create a search component that updates the URL as users type:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ArticleSearch.bx
class extends="cbwire.models.Component" {
    data = {
        "search": "",
        "category": "",
        "results": []
    };
    
    queryString = ["search", "category"];
    
    function mount() {
        performSearch();
    }
    
    function updated() {
        performSearch();
    }
    
    function performSearch() {
        if (data.search.len() > 2) {
            // Simulate search results
            data.results = [
                {"title": "Article 1", "category": "Tech"},
                {"title": "Article 2", "category": "News"}
            ];
        } else {
            data.results = [];
        }
    }
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ArticleSearch.cfc
component extends="cbwire.models.Component" {
    data = {
        "search" = "",
        "category" = "",
        "results" = []
    };
    
    queryString = ["search", "category"];
    
    function mount() {
        performSearch();
    }
    
    function updated() {
        performSearch();
    }
    
    function performSearch() {
        if (len(data.search) > 2) {
            // Simulate search results
            data.results = [
                {"title" = "Article 1", "category" = "Tech"},
                {"title" = "Article 2", "category" = "News"}
            ];
        } else {
            data.results = [];
        }
    }
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="BoxLang" %}
```html
<!-- wires/articleSearch.bxm -->
<bx:output>
<div>
    <h1>Article Search</h1>
    
    <div class="search-form">
        <input wire:model="search" 
               type="search" 
               placeholder="Search articles...">
        
        <select wire:model="category">
            <option value="">All Categories</option>
            <option value="tech">Technology</option>
            <option value="news">News</option>
            <option value="sports">Sports</option>
        </select>
    </div>
    
    <div class="results">
        <h2>Results (#results.len()#)</h2>
        <bx:loop array="#results#" item="article">
            <div class="article">
                <h3>#article.title#</h3>
                <span class="category">#article.category#</span>
            </div>
        </bx:loop>
    </div>
</div>
</bx:output>
```
{% endtab %}

{% tab title="CFML" %}
```html
<!-- wires/articleSearch.cfm -->
<cfoutput>
<div>
    <h1>Article Search</h1>
    
    <div class="search-form">
        <input wire:model="search" 
               type="search" 
               placeholder="Search articles...">
        
        <select wire:model="category">
            <option value="">All Categories</option>
            <option value="tech">Technology</option>
            <option value="news">News</option>
            <option value="sports">Sports</option>
        </select>
    </div>
    
    <div class="results">
        <h2>Results (#arrayLen(results)#)</h2>
        <cfloop array="#results#" item="article">
            <div class="article">
                <h3>#article.title#</h3>
                <span class="category">#article.category#</span>
            </div>
        </cfloop>
    </div>
</div>
</cfoutput>
```
{% endtab %}
{% endtabs %}

When users visit `/articles?search=javascript&category=tech`, the component automatically initializes with those values and performs the search.

## Custom Query String Keys

Map data properties to different query parameter names:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
// wires/ProductFilter.bx
class extends="cbwire.models.Component" {
    data = {
        "searchTerm": "",
        "priceRange": "",
        "inStock": false
    };
    
    queryString = {
        "searchTerm": {"as": "q"},
        "priceRange": {"as": "price"},
        "inStock": {"as": "available"}
    };
}
```
{% endtab %}

{% tab title="CFML" %}
```javascript
// wires/ProductFilter.cfc
component extends="cbwire.models.Component" {
    data = {
        "searchTerm" = "",
        "priceRange" = "",
        "inStock" = false
    };
    
    queryString = {
        "searchTerm" = {"as" = "q"},
        "priceRange" = {"as" = "price"},
        "inStock" = {"as" = "available"}
    };
}
```
{% endtab %}
{% endtabs %}

This creates URLs like `/products?q=laptop&price=500-1000&available=true` instead of using the full property names.

## Excluding Empty Values

Prevent empty values from appearing in the URL:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
queryString = {
    "search": {"except": ""},
    "category": {"except": ""},
    "page": {"except": 1}
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
queryString = {
    "search" = {"except" = ""},
    "category" = {"except" = ""},
    "page" = {"except" = 1}
};
```
{% endtab %}
{% endtabs %}

When `search` is empty, `category` is empty, or `page` equals 1, those parameters won't appear in the URL.

## History Management

Control browser history behavior:

{% tabs %}
{% tab title="BoxLang" %}
```javascript
queryString = {
    "search": {"history": false},  // Don't create history entries
    "page": {"history": true}      // Create history entries (default)
};
```
{% endtab %}

{% tab title="CFML" %}
```javascript
queryString = {
    "search" = {"history" = false},  // Don't create history entries
    "page" = {"history" = true}      // Create history entries (default)
};
```
{% endtab %}
{% endtabs %}

Use `"history": false` for rapidly changing values like search terms to avoid cluttering browser history.

{% hint style="info" %}
Query string synchronization happens automatically when data properties change. You don't need to manually update the URL.
{% endhint %}

{% hint style="warning" %}
Only properties listed in the `queryString` array or object will be synchronized with the URL. Other data properties remain internal to the component.
{% endhint %}
