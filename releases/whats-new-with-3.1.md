# What's New With 3.1

09/25/2023

## New Features

### Auto-Include CBWIRE Assets

CBWIRE 3.1 introduces automatic inclusion of styles and scripts, eliminating the need to manually call `wireScripts()` and `wireStyles()` in your templates.

```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "autoIncludeAssets" = true  // Default: true
    }
};
```

Previously required manual inclusion:
```html
<!-- Old way - no longer needed -->
<head>
    #wireStyles()#
</head>
<body>
    <!-- content -->
    #wireScripts()#
</body>
```

Now assets are automatically included when components are rendered.

### Enhanced Lifecycle Hooks

New `onUpdate()` and `onUpdateProperty()` lifecycle methods provide granular control over component updates.

```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "John Doe",
        "email" = "john@example.com",
        "lastUpdated" = now()
    };
    
    function onUpdate() {
        // Called after any component update
        data.lastUpdated = now();
        logActivity("Profile updated");
    }
    
    function onUpdateProperty(propertyName, newValue, oldValue) {
        // Called when specific properties change
        if (propertyName == "email") {
            sendEmailVerification(newValue);
        }
    }
    
    function updateProfile() {
        // This will trigger onUpdate() and onUpdateProperty()
        data.name = "Jane Doe";
        data.email = "jane@example.com";
    }
}
```

### Child Component Refresh

Parent components can now refresh all child components using the new refresh functionality.

```javascript
// wires/Dashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "stats" = {},
        "notifications" = []
    };
    
    function refreshAllData() {
        // Refresh parent data
        data.stats = getLatestStats();
        data.notifications = getNotifications();
        
        // Refresh all child components
        refresh();
    }
}
```

```html
<!-- wires/dashboard.cfm -->
<cfoutput>
<div class="dashboard">
    <button wire:click="refreshAllData">Refresh All</button>
    
    <div class="stats">
        #wire("StatsComponent")#
    </div>
    
    <div class="notifications">
        #wire("NotificationComponent")#
    </div>
</div>
</cfoutput>
```

### UDF Access in Templates

Components can now call CBWIRE User Defined Functions directly from templates, expanding beyond computed properties.

```javascript
// wires/ProductCatalog.cfc
component extends="cbwire.models.Component" {
    data = {
        "products" = [],
        "category" = "all"
    };
    
    function onMount() {
        data.products = getProducts();
    }
    
    function formatPrice(price) {
        return dollarFormat(price);
    }
    
    function isOnSale(product) {
        return product.salePrice < product.regularPrice;
    }
    
    function getCategoryName(categoryId) {
        return getCategory(categoryId).name;
    }
}
```

```html
<!-- wires/productCatalog.cfm -->
<cfoutput>
<div class="product-catalog">
    <h2>Category: #getCategoryName(category)#</h2>
    
    <cfloop array="#products#" index="product">
        <div class="product #isOnSale(product) ? 'on-sale' : ''#">
            <h3>#product.name#</h3>
            <p class="price">
                <cfif isOnSale(product)>
                    <span class="regular">#formatPrice(product.regularPrice)#</span>
                    <span class="sale">#formatPrice(product.salePrice)#</span>
                <cfelse>
                    #formatPrice(product.regularPrice)#
                </cfif>
            </p>
        </div>
    </cfloop>
</div>
</cfoutput>
```

### Single File Components Improvements

Enhanced single file component handling with better caching and collision prevention.

```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "cacheSingleFileComponents" = true  // Cache compiled SFCs
    }
};
```

Single file components now automatically clear on `fwreinit` and have improved file name collision handling for high-traffic applications.

### Enhanced onMount() Parameters

Support for `params` as an alias for `parameters` in `onMount()` method calls.

```javascript
// wires/ReportViewer.cfc
component extends="cbwire.models.Component" {
    data = {
        "report" = {},
        "reportId" = ""
    };
    
    // Both 'parameters' and 'params' work
    function onMount(params = {}) {
        if (structKeyExists(params, "reportId")) {
            data.reportId = params.reportId;
            data.report = getReport(params.reportId);
        }
    }
}
```

### Template Helper Access

Templates can now access ColdBox.cfc application helpers and module helpers directly.

```javascript
// ColdBox.cfc
function applicationHelper() {
    return {
        "formatDate" = function(date) {
            return dateFormat(date, "mm/dd/yyyy");
        },
        "truncateText" = function(text, length = 50) {
            return left(text, length) & (len(text) > length ? "..." : "");
        }
    };
}
```

```html
<!-- wires/blogPost.cfm -->
<cfoutput>
<article class="blog-post">
    <h2>#title#</h2>
    <p class="meta">Published: #formatDate(publishedDate)#</p>
    <p class="excerpt">#truncateText(content, 150)#</p>
</article>
</cfoutput>
```

### Reset Methods Enhancement

New `resetExcept()` method allows resetting all data properties except specified ones.

```javascript
// wires/UserForm.cfc
component extends="cbwire.models.Component" {
    data = {
        "name" = "",
        "email" = "",
        "phone" = "",
        "formSubmitted" = false,
        "errors" = {}
    };
    
    function clearForm() {
        // Reset everything except submission status
        resetExcept("formSubmitted");
    }
    
    function clearFormKeepEmail() {
        // Reset everything except email and submission status
        resetExcept(["email", "formSubmitted"]);
    }
}
```

## Enhancements

### Performance Improvements

- Faster rendering with optimized view caching and lookups
- Improved single file component compilation and caching
- Better memory management for high-traffic applications

### Template Variable Cleanup

- Removed unnecessary template variables to prevent collisions
- Cleaner template scope with better variable isolation
- Improved debugging experience

### Child Component Management

- Fixed child components not re-rendering on follow-up requests
- Better tracking and lifecycle management for nested components
- Improved parent-child communication

## Bug Fixes

### Template Rendering

- **Fixed HTML comments breaking re-renders**: HTML comments in templates no longer interfere with component re-rendering
- **Fixed child component rendering**: Child components now properly re-render on subsequent requests

### Single File Components

- **Fixed file name collisions**: Improved handling of SFC file names in high-traffic applications
- **Automatic cleanup**: Compiled SFCs are now properly cleared on `fwreinit`

## Breaking Changes

This release maintains backward compatibility with existing CBWIRE 3.x applications. The automatic asset inclusion is enabled by default but can be disabled if needed.
