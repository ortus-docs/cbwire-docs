---
description: >-
  You can use CBWIRE alongside ContentBox to build modernize, reactive
  applications with a powerful CMS included.
---

# ContentBox CMS

[ContentBox](https://contentbox.ortusbooks.com/) is a professional open source _hybrid_ modular CMS (Content Management System) that allows you to easily build websites, blogs, wikis, complex web applications and even power mobile or cloud applications. Built with a secure and flexible modular core, designed to scale, and combined with world-class support, ContentBox will get your projects out the door in no time.

## Installation

{% hint style="info" %}
You need to install CBWIRE version 2.3.3 or later in order to be able to use CBWIRE and ContentBox.
{% endhint %}

In the root of your ContentBox application, install CBWIRE using [CommandBox](https://commandbox.ortusbooks.com/). The example below installs the latest bleeding-edge version.

```
box install cbwire@be
```

{% hint style="info" %}
Once installed, you will need to restart your app using ?fwreinit=\[your secret key].
{% endhint %}

{% hint style="warning" %}
Ensure that CBWIRE is installed under **/modules/cbwire** from the root of your project. It should be installed there by default. Installing CBWIRE elsewhere can cause errors within a ContentBox application.
{% endhint %}

## Theme Integration

**You will need to use a custom theme in order to integrate with CBWIRE**.

If you are wanting to use a theme that is included with ContentBox or one that you've pulled down from ForgeBox, you can copy the theme code over to the folder **modules\_app/contentbox-custom/\_themes**.

Once CBWIRE is installed, you should have access to the three core included methods and should be able to reference these within your layout and theme templates.

* **wireStyles()** - Pulls in required styling for CBWIRE
* **wireScripts()** - Pulls in required JavaScript assets for CBWIRE/Livewire
* **wires()** - Includes a [Wire](../essentials/creating-components.md) (reactive UI element) you create

In your theme layout, you need to place `wireStyles()` within your \<head> tags, `wireScripts()` before the end of \</body>, and you can call `wire()` to include your reactive UI Wires where needed.

```html
<!--- modules_app/contentbox-custom/_themes/cbwireTheme/layouts/blog.cfm --->
<cfoutput>
<!--- Global Layout Arguments --->
<cfparam name="args.print" default="false">
<cfparam name="args.sidebar" default="true">

<!DOCTYPE html>
<html lang="en">
<head>
	<!--- Page Includes --->
	#cb.quickView( "_blogIncludes" )#

	<!--- ContentBoxEvent --->
	#cb.event( "cbui_beforeHeadEnd" )#

	<!--- PULL IN CBWIRE STYLING --->
	#wireStyles()#
</head>
<body>
	<!--- ContentBoxEvent --->
	#cb.event( "cbui_afterBodyStart" )#

	<!--- Header --->
	#cb.quickView( '_header' )#

	<!--- ContentBoxEvent --->
	#cb.event( "cbui_beforeContent" )#

	<!--- Main View --->
	#cb.mainView( args=args )#
	
	<!--- INCLUDE REACTIVE NEWSLETTER SIGNUP COMPONENT --->
	#wire( "NewsletterSignup", {
		validate: true
	} )#

	<!--- ContentBoxEvent --->
	#cb.event( "cbui_afterContent" )#

	#cb.quickView( view='_footer' )#

	<!--- ContentBoxEvent --->
	#cb.event( "cbui_beforeBodyEnd" )#

	<!--- PULL IN CBWIRE JAVASCRIPT ASSETS --->
	#wireScripts()#
</body>
</html>
</cfoutput>
```

You can also reference the wire() method within your views.

```html
<!--- modules_app/contentbox-custom/_themes/cbwireTheme/views/index.cfm --->
<div class="row">
    <div class="col-12">
        #wire( "ContactForm" )#
    </div>
</div>
```
