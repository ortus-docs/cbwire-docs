# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the documentation repository for CBWIRE, a ColdBox module that brings reactive UI capabilities to BoxLang and CFML applications using Livewire and Alpine.js. The repository contains GitBook-formatted documentation written in Markdown.

## Architecture

The documentation is structured as a GitBook project with the following organization:

- **Root files**: `README.md` (introduction), `SUMMARY.md` (table of contents), configuration files
- **Core sections**:
  - `the-essentials/` - Core concepts (components, templates, data binding, actions, events, etc.)
  - `features/` - Advanced features (Alpine.js integration, file uploads, form validation, etc.) 
  - `template-directives/` - Wire directive documentation (wire:click, wire:model, wire:loading, etc.)
  - `releases/` - Version release notes and changelogs
  - `advanced/` - Troubleshooting and advanced topics

## Content Structure

- Documentation uses GitBook syntax with tabs for BoxLang vs CFML code examples
- Most code examples are provided in both BoxLang (.bx/.bxm) and CFML (.cfc/.cfm) formats
- Template files use `.bxm` extension for BoxLang and `.cfm` for CFML
- Component files use `.bx` extension for BoxLang and `.cfc` for CFML

## Key Documentation Patterns

- Components extend `cbwire.models.Component`
- Components are placed in `./wires/` folder by default
- Templates use `wire:` directives for reactive behavior
- Data properties are defined in a `data` struct
- Actions are public methods that can be called from templates
- Integration with Alpine.js using `$wire` object

## Development Notes

- No build system - this is a static documentation repository
- Content is managed through GitBook's interface or direct markdown editing
- Documentation covers CBWIRE versions 2.x through 4.x
- Examples assume ColdBox framework integration

## Content Guidelines

- **Language Priority**: Always mention BoxLang first, then CFML (e.g., ".bx for BoxLang, .cfc for CFML")
- **Writing Style**: Documentation should be thorough but short, clear, and elegant
- **File Extensions**: Include file extensions elegantly when describing components and templates
- info, hint, and warning boxes should be shown as after all paragraphs in a section for consistency throughout. all documentation needs to be stored into logical structure that is easy to follow and we should always show plently of code examples. all code examples should be boxlang first and CFML second and you should use the code tabs that we are already using in the code. we want all documentation to have a consistent writing style and voice with a perfect blend of technical but approachable.
- things like class versus component definitions in boxlang vs cfml warrant showing separate tabs, the first should be BoxLang and the second CFML. However, some code examples that don't really have much different whatsoever like showing simple structs, arrays, or variable assignments don't need to have a BoxLang and CFML tab to toggle between. In those instances we don't need to different language tabs.
- when referencing data properties in component code, use 'data.' instead of 'variables.data.'
- when referencing data properties in templates, access them directly without the 'data.' prefix (e.g., #variableName# not #data.variableName#)
- in code examples always use modern coding techniques such as member functions .each(), .reduce(), .map(), .filter() .len(), .trim(), Arrays are array.append();
- always avoid for loops if possible. use .each() instead
- wire:each doesn't exists. use proper template looping. boxlang would use <bx:loop array=""></bx:loop> and CFML would use <cfloop array=""></cfloop>

## Template Directive Documentation Patterns

- Remove "Example:" headers - integrate examples naturally into the flow
- Start with clear explanation of what the directive does and why it's useful
- Provide practical, real-world examples that demonstrate value
- Include modifier documentation with clear explanations
- Show both simple and advanced usage patterns
- End with limitations or important considerations
- Use consistent structure: Overview → Basic Usage → Modifiers → Advanced Examples → Notes
- **CRITICAL**: Only document features and details that exist in the original documentation - never add assumed technical details, CSS classes, timing values, or implementation specifics that aren't explicitly mentioned in the source material
- Livewire (and therefore CBWIRE) automatically disables forms and submit buttons during submission - don't show manual disabled attributes unless there's a specific reason
- always avoid for loops if possible. use .each() instead
- wire:each doesn't exists. use proper template looping. boxlang would use <bx:loop array=""></bx:loop> and CFML would use <cfloop array=""></cfloop>
- when creating docs for template directive pages, refer to wire:transition page as this is well done and i'd like all pages to be consistent in overall structure and format
- template directive pages should have only 1 practical example that showcases as many features of the directive as possible
- you have to use data.variableName when accessing data properties in compnoents or classes.
- to relocate users always use redirect() not relocate() - redirect("/some-uri") or redirect("https://url.com") or redirect("/some-uri", true) for wire:navigate
- form validation should use the built-in cbValidation documented at https://cbwire.ortusbooks.com/features/form-validation instead of manual validation
- 1 single practicle example should be enough for template directives
- i prefer to always have one single practical example under the heading "Basic Usage" on each template directive page
- each template directive page should always have a 'What wire:directive Does' section
- basic usage examples on template directive pages should be short and concise and focused on the directive at hand
- basic usage examples should be VERY SHORT - just enough to demonstrate the directive functionality, not complex scenarios
- basic usage should always go after introdocutory paragraphs
- keep basic usage examples minimal - you're only trying to show the user the specific directive being documented
- when i tell you to improve a template directive page (wire:), resturcture the page to match other pages such as wire:transition but make sure to not hallucinate things that don't exist such as available modifiers. use the original source material to check your work
- always check yourself to see if you are repeating yourself.