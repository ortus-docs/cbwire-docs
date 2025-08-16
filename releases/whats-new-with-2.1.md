# What's New With 2.1

11/26/2022

## New Features

### Direct HTML Rendering

CBWIRE 2.1 introduces the ability to return HTML directly from the `onRender()` method, eliminating the need for separate template files for simple components.

```javascript
// wires/SimpleAlert.cfc
component extends="cbwire.models.Component" {
    data = {
        "message" = "",
        "type" = "info",
        "visible" = true
    };
    
    function mount(params = {}) {
        data.message = params.message ?: "Default message";
        data.type = params.type ?: "info";
    }
    
    function onRender() {
        // Return HTML directly without needing a .cfm file
        if (!data.visible) {
            return "";
        }
        
        var alertClass = "alert alert-" & data.type;
        var html = '
        <div class="#alertClass#">
            <button wire:click="dismiss" class="close">&times;</button>
            #data.message#
        </div>';
        
        return html;
    }
    
    function dismiss() {
        data.visible = false;
    }
}
```

This feature is particularly useful for:
- Simple notification components
- Dynamic content that doesn't warrant a separate template file
- Components with conditional rendering logic
- Programmatically generated markup

```javascript
// wires/DynamicCard.cfc
component extends="cbwire.models.Component" {
    data = {
        "items" = [],
        "cardType" = "default"
    };
    
    function onRender() {
        var html = '<div class="card card-#data.cardType#">';
        
        for (var item in data.items) {
            html &= '<div class="card-item">';
            html &= '<h4>#item.title#</h4>';
            html &= '<p>#item.description#</p>';
            html &= '</div>';
        }
        
        html &= '</div>';
        return html;
    }
    
    function addItem(title, description) {
        arrayAppend(data.items, {
            "title" = title,
            "description" = description
        });
    }
}
```

## Bug Fixes

### Computed Properties

- **Fixed empty return values**: Computed properties that return no value no longer cause errors
- **Better error handling**: Improved error messages when computed properties fail

```javascript
// This now works correctly in 2.1
component extends="cbwire.models.Component" {
    function getDisplayName() {
        // This won't error if it returns nothing
        if (structKeyExists(data, "name")) {
            return data.name;
        }
        // Implicit return of empty value handled gracefully
    }
}
```

### Nested Component Rendering

- **Fixed template rendering**: Nested components now properly render their individual templates instead of only the last one
- **Fixed component isolation**: Nested components no longer interfere with each other's rendering
- **Improved nesting support**: Components can now be deeply nested without rendering issues

```html
<!-- This now works correctly -->
<cfoutput>
<div class="parent-component">
    #wire("ChildComponent1")#
    #wire("ChildComponent2")# 
    #wire("NestedContainer")#
</div>
</cfoutput>
```

### Template Data Handling

- **Fixed struct value preservation**: Struct values in templates are no longer replaced with empty strings
- **Better data type handling**: Improved handling of complex data types in template rendering
- **Preserved variable scope**: Template variables maintain their proper types and values

### ColdBox Integration

- **Subdirectory support**: CBWIRE now works correctly with ColdBox applications installed in subdirectories
- **Version compatibility**: Fixed rendering errors that occurred with the latest ColdBox versions
- **Improved path resolution**: Better handling of application paths and module locations

### Component Lifecycle

- **Rendering reliability**: Fixed various scenarios where components would fail to render
- **State management**: Improved component state handling during complex rendering scenarios
- **Error recovery**: Better error handling and recovery during component initialization

## Enhancements

### Performance Improvements

- Faster rendering for components using `onRender()` method
- Reduced overhead for simple components that don't need template files
- Better memory management for nested component hierarchies

### Developer Experience

- Clearer error messages for rendering issues
- Better debugging information for failed component initialization
- Improved development workflow for simple components

### Template System

- More reliable template resolution
- Better handling of edge cases in component nesting
- Improved variable scope management

## Breaking Changes

This release maintains full backward compatibility with existing CBWIRE 2.0 applications. No breaking changes were introduced in version 2.1.

