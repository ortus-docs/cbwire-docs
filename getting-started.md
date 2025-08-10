---
description: With CommandBox, you can start building reactive CFML apps in seconds.
---

# Getting Started

## Requirements

* Adobe ColdFusion 2018+ or Lucee 5+
* ColdBox 6+

## Installation

Install [CommandBox](https://www.ortussolutions.com/products/commandbox).

Within the root of your project, run:

```bash
box install cbwire
```

If you want the latest bleeding edge, run:

```bash
box install cbwire@be
```

## Layout Setup

You need to add references for `wireStyles()` and `wireScripts()` to your layout file.

```html
<!--- ./layouts/Main.cfm --->
<cfoutput>
<!DOCTYPE html>
<html lang="en">
    <head>
        <title>CBWIRE Example</title>
        #wireStyles()#
    </head>
    <body>
        
        <!--- JavasScript references below --->
        #wireScripts()#
    </body>
</html>
</cfoutput>
```

{% hint style="warning" %}
CBWIRE will not work if you do not add these to your layout.
{% endhint %}

## Inserting Wires

You can insert [Wires](essentials/creating-components.md) anywhere in your layout or ColdBox views using `wire( componentName )`.

```html
<!--- ./layouts/Main.cfm --->
<body>
    <!--- Insert our task list wire here --->
    #wire( "TaskList" )#
            
    <!--- JavasScript references below --->
    #wireScripts()#
</body>
```

```html
<!--- ./views/someview.cfm --->
<cfoutput>
    <div>#wire( "TaskList" )#</div>
</cfoutput>
```
