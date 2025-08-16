# What's New With 2.0

08/30/2022

## New Features

### File Upload Support

CBWIRE 2.0 introduces comprehensive file upload capabilities with built-in handling for single and multiple files.

```javascript
// wires/FileUploader.cfc
component extends="cbwire.models.Component" {
    data = {
        "uploadedFile" = "",
        "uploadedFiles" = [],
        "uploadProgress" = 0
    };
    
    function processUpload() {
        if (structKeyExists(data, "uploadedFile") && len(data.uploadedFile)) {
            // Handle single file upload
            var uploadPath = expandPath("./uploads/");
            fileMove(data.uploadedFile, uploadPath & data.uploadedFile.getClientFileName());
            
            data.uploadProgress = 100;
        }
    }
    
    function processMultipleUploads() {
        if (arrayLen(data.uploadedFiles)) {
            var uploadPath = expandPath("./uploads/");
            
            for (var file in data.uploadedFiles) {
                fileMove(file, uploadPath & file.getClientFileName());
            }
            
            data.uploadProgress = 100;
        }
    }
}
```

```html
<!-- wires/fileUploader.cfm -->
<cfoutput>
<div class="upload-container">
    <form wire:submit="processUpload">
        <label for="file">Choose File:</label>
        <input type="file" wire:model="uploadedFile" id="file">
        
        <cfif uploadProgress GT 0>
            <div class="progress">
                <div class="progress-bar" style="width: #uploadProgress#%">#uploadProgress#%</div>
            </div>
        </cfif>
        
        <button type="submit">Upload File</button>
    </form>
    
    <form wire:submit="processMultipleUploads">
        <label for="files">Choose Multiple Files:</label>
        <input type="file" wire:model="uploadedFiles" id="files" multiple>
        <button type="submit">Upload Files</button>
    </form>
</div>
</cfoutput>
```

### Component Testing

Write comprehensive unit tests for your CBWIRE components with the new testing framework.

```javascript
// tests/specs/integration/ComponentTest.cfc
component extends="cbwire.testing.BaseWireSpec" {
    
    function testCounterComponent() {
        var comp = visitComponent("Counter")
            .assertSee("0")
            .call("increment")
            .assertSee("1")
            .call("increment")
            .assertSee("2")
            .call("reset")
            .assertSee("0");
    }
    
    function testFormSubmission() {
        var comp = visitComponent("ContactForm")
            .set("name", "John Doe")
            .set("email", "john@example.com")
            .set("message", "Test message")
            .call("submitForm")
            .assertSee("Thank you")
            .assertDontSee("Please fill");
    }
    
    function testDataPersistence() {
        var comp = visitComponent("UserProfile", { "userId": 123 })
            .assertSee("John Doe")
            .set("firstName", "Jane")
            .call("updateProfile")
            .assertSee("Jane Doe")
            .assertDontSee("John Doe");
    }
}
```

### Custom Wires Directory

Override the default `wires` folder location to organize components according to your application structure.

```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "wiresDirectory" = "/custom/components/path"
    }
};
```

```javascript
// Alternative: Set custom location per component
component extends="cbwire.models.Component" {
    this.template = "/custom/templates/MyComponent.cfm";
}
```

### Skip Rendering with noRender()

Prevent template rendering when you only need to update data without returning HTML.

```javascript
// wires/APIHandler.cfc
component extends="cbwire.models.Component" {
    data = {
        "status" = "idle",
        "result" = {}
    };
    
    function processAPICall() {
        data.status = "processing";
        
        try {
            data.result = callExternalAPI();
            data.status = "success";
        } catch (any e) {
            data.status = "error";
            data.result = { "error" = e.message };
        }
        
        // Skip template rendering for AJAX-only updates
        noRender();
    }
}
```

### Dirty Property Tracking

Track which properties have changed since the last render for optimized updates.

```javascript
// wires/FormTracker.cfc
component extends="cbwire.models.Component" {
    data = {
        "formData" = {},
        "hasChanges" = false,
        "changedFields" = []
    };
    
    function onUpdated() {
        // Track which properties are dirty
        var dirtyProps = getDirtyProperties();
        
        if (arrayLen(dirtyProps)) {
            data.hasChanges = true;
            data.changedFields = dirtyProps;
        }
    }
    
    function saveChanges() {
        if (data.hasChanges) {
            // Only save changed fields
            for (var field in data.changedFields) {
                updateFieldInDatabase(field, data.formData[field]);
            }
            
            data.hasChanges = false;
            data.changedFields = [];
        }
    }
}
```

### Computed Property Optimization

Computed properties now run only once per request during rendering for better performance.

```javascript
// wires/Dashboard.cfc
component extends="cbwire.models.Component" {
    data = {
        "users" = [],
        "orders" = []
    };
    
    // This expensive calculation runs only once per render
    function getTotalRevenue() {
        var total = 0;
        
        for (var order in data.orders) {
            total += order.amount;
        }
        
        return dollarFormat(total);
    }
    
    // This also runs once and caches the result
    function getUserStats() {
        return {
            "total" = arrayLen(data.users),
            "active" = data.users.filter(function(user) {
                return user.status == "active";
            }).len(),
            "premium" = data.users.filter(function(user) {
                return user.isPremium;
            }).len()
        };
    }
}
```

### Dependency Injection Support

Components now support ColdBox's dependency injection system.

```javascript
// wires/UserManager.cfc
component extends="cbwire.models.Component" {
    
    property name="userService" inject="UserService";
    property name="emailService" inject="EmailService";
    property name="settings" inject="coldbox:moduleSettings:cbwire";
    
    data = {
        "users" = [],
        "selectedUser" = {}
    };
    
    function mount() {
        data.users = userService.getAllUsers();
    }
    
    function sendWelcomeEmail(userId) {
        var user = userService.getUser(userId);
        emailService.sendWelcomeEmail(user.email, user.firstName);
    }
}
```

### Enhanced Turbo Support

Enable single-page application functionality with improved Turbo integration.

```javascript
// config/ColdBox.cfc
moduleSettings = {
    "cbwire" = {
        "enableTurbo" = true,
        "moduleRootURI" = "/custom/cbwire/path"
    }
};
```

## Enhancements

### Template Path Flexibility

Set custom template paths for components using the `this.template` property.

```javascript
// wires/CustomComponent.cfc
component extends="cbwire.models.Component" {
    this.template = "/custom/views/special-template.cfm";
    
    data = {
        "content" = "Custom template content"
    };
}
```

### Null Value Support

Data properties now properly support `null` as a valid value.

```javascript
// wires/UserProfile.cfc
component extends="cbwire.models.Component" {
    data = {
        "avatar" = null,  // Null is now a valid value
        "bio" = null,
        "website" = null
    };
    
    function clearAvatar() {
        data.avatar = null;  // Explicitly set to null
    }
}
```

### Livewire JavaScript Upgrade

Updated to Livewire JS v2.10.6 for improved client-side functionality and bug fixes.

### Performance Optimizations

- Disabled browser caching for XHR responses to ensure fresh data
- Trimmed XHR payload by removing unnecessary data
- Moved internal methods to Engine object to prevent naming conflicts
- Added security by rejecting XHR requests without proper headers

## Bug Fixes

### Component State Management

- **Fixed reset() errors**: Calling `reset("someProperty")` no longer causes errors
- **Preserved component IDs**: Component IDs are now preserved across renders to avoid DOM diffing issues
- **Better parameter handling**: Fixed missing parameters in update method calls

### Browser Compatibility

- **Fixed back button issues**: Browser back button now works correctly with CBWIRE components
- **Improved navigation**: Better handling of browser history and navigation states

### Data Integrity

- **Parameter validation**: Ensured `params` is properly passed as an array
- **Method parameter handling**: Fixed missing parameters in component method calls

## Breaking Changes

This is a major version release with some breaking changes:

### Component Structure

Some internal method names have changed to prevent conflicts. If you were extending or overriding internal CBWIRE methods, you may need to update your code.

### XHR Security

XHR requests now require the `X-Livewire` header. This improves security but may affect custom AJAX implementations.

### Template Resolution

The default template resolution has been improved. If you have custom template path logic, verify it still works correctly with the new resolution system.
