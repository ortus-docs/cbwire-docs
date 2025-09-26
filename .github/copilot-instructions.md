# CBWIRE Documentation Repository

This is the official documentation repository for CBWIRE, a ColdBox module that brings reactive UI capabilities to BoxLang and CFML applications using Livewire and Alpine.js.

## Repository Purpose

CBWIRE enables developers to build modern, reactive web applications without complex frontend frameworks or APIs. This documentation covers:

- Core concepts and components
- Getting started guides
- Configuration options
- Template directives and features
- Advanced usage patterns
- CLI tools and scaffolding

## Documentation Structure

The documentation is organized using GitBook format with the following structure:

- `README.md` - Main introduction and overview
- `SUMMARY.md` - Table of contents defining the structure
- `getting-started.md` - Installation and basic setup
- `configuration.md` - Module configuration options
- `cbwire-cli.md` - CLI tool documentation
- `the-essentials/` - Core concepts (components, templates, properties, actions, events, etc.)
- `features/` - Advanced features (single-file components, lazy loading, validation, etc.)
- `template-directives/` - Wire directives (wire:click, wire:model, etc.)
- `releases/` - Version release notes
- `advanced/` - Troubleshooting and advanced topics

## Language Support

CBWIRE supports both BoxLang and CFML. Documentation should include examples for both languages where relevant:

- Use tabs to separate BoxLang and CFML examples
- BoxLang examples use `.bx` extensions and `bx:` tags
- CFML examples use `.cfc/.cfm` extensions and `cf` tags

## Content Guidelines

### Code Examples
- Always provide working, runnable examples
- Include both BoxLang and CFML versions when showing language-specific syntax
- Use proper syntax highlighting
- Keep examples simple but practical

### Writing Style
- Use clear, concise language
- Write in an instructional, helpful tone
- Include practical use cases and scenarios
- Provide context for when and why to use features

### Documentation Standards
- Follow GitBook markdown conventions
- Use hints/callouts for important information:
  - `{% hint style="info" %}` for helpful tips
  - `{% hint style="warning" %}` for important warnings
  - `{% hint style="danger" %}` for critical notes
- Structure content with clear headings
- Include cross-references to related topics

## Key Concepts to Understand

- **Components**: Server-side classes that manage state and logic
- **Templates**: HTML templates with reactive directives
- **Wire Directives**: Special attributes for binding data and handling events
- **Reactive Data**: Properties that automatically sync between server and client
- **Actions**: Methods that handle user interactions
- **Lifecycle Events**: Hooks for component initialization and updates
- **Alpine.js Integration**: Client-side JavaScript framework integration

## File Extensions and Conventions

- Component files: `.bx` (BoxLang) or `.cfc` (CFML)
- Template files: `.bxm` (BoxLang) or `.cfm` (CFML)
- Configuration: ColdBox application settings
- CLI: CommandBox-based tooling

When contributing to or modifying this documentation, maintain consistency with existing patterns and ensure all examples are tested and functional.