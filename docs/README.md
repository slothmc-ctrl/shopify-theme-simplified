# Theme Documentation

This directory contains comprehensive documentation for the Shopify theme components and systems.

## Documentation Overview

This documentation provides detailed guides, references, and technical information to help developers understand, customize, and extend the theme. Documentation is organized by component and system, with both user-facing guides and technical architecture details.

### Navigation
- Use the sections below to find documentation for specific components
- Each document includes cross-references to related files and sections
- Quick reference guides provide fast lookups for common tasks

## Button Component Documentation

### ✅ [BUTTON_COMPONENT_SYSTEM.md](BUTTON_COMPONENT_SYSTEM.md)
Complete reference guide for the button component system. Covers component API, variant architecture, CSS class mappings, theme settings integration, usage patterns, and migration guidance.

### ✅ [BUTTON_QUICK_REFERENCE.md](BUTTON_QUICK_REFERENCE.md)
Quick lookup guide for common button patterns. Includes parameter cheat sheets, variant references, and copy-paste ready code snippets.

### ✅ [BUTTON_ARCHITECTURE.md](BUTTON_ARCHITECTURE.md)
Technical architecture and design decisions for the button system. Explains internal workings, data flow, and extensibility points.

## Animation System Documentation

### 📋 Planned: Animation System Reference
Documentation for the animation system including `theme.HoverButton`, `theme.MagnetButton`, and `theme.RevealButton` classes. Will be added in subsequent phases.

## Component Catalog

### 📋 Planned: Component Catalog
Comprehensive catalog of all theme components including sections, snippets, and reusable elements. Will be added in subsequent phases.

## Theme Settings Reference

### 📋 Planned: Theme Settings Reference
Complete reference for all theme customization options. For now, see `config/settings_schema.json` for the raw settings structure.

## Contributing Guidelines

### When to Update Documentation
- After adding new components or features
- When modifying existing component APIs
- When changing theme settings or configuration
- When updating CSS class names or Liquid parameters

### Documentation Style Guide
- Use Markdown with clear heading hierarchy
- Include code blocks with syntax highlighting
- Use tables for reference information
- Add cross-references between related documents
- Keep technical documents separate from user guides

### Adding New Documentation
1. Create new `.md` files in this directory
2. Update this README to include the new document
3. Add appropriate status badges (✅ Complete, 🚧 In Progress, 📋 Planned)
4. Include brief descriptions and links

## Quick Links

- [Shopify Liquid Documentation](https://shopify.dev/docs/themes/liquid/reference)
- [Theme Architecture Overview](BUTTON_ARCHITECTURE.md)
- [Component Usage Examples](BUTTON_COMPONENT_SYSTEM.md#usage-examples)
- [Settings Schema](../config/settings_schema.json)