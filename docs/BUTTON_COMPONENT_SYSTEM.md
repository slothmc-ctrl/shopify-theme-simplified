# Button Component System Reference

## Overview

The button component is a unified interface for rendering buttons throughout the Shopify theme. Located at `snippets/button.liquid`, it provides a consistent API for creating buttons with various styles, sizes, and behaviors.

**Note: JavaScript-based hover animations have been removed.** The component now uses only modern CSS variants for all button effects, providing better performance and accessibility.

The component supports both legacy and modern variant systems for backward compatibility while enabling enhanced styling options.

## Component API Reference

### Core Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `label` | String | Required | The button text content |
| `link` | String | - | URL for link-style buttons |
| `element_type` | String | `"link"` | HTML element: `"link"`, `"button"`, or `"submit"` |
| `button_type` | String | `"button"` | Button type attribute: `"button"`, `"submit"`, `"reset"` |
| `form_id` | String | - | Associated form ID for submit buttons |
| `disabled` | Boolean | `false` | Whether the button is disabled |

### Variant Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `variant` | String | `"primary"` | Modern variant: `"primary"`, `"secondary"`, `"success"`, `"danger"`, `"ghost"`, `"outline"`, `"text-link"`, `"icon"` |
| `style` | String | - | Legacy style: `"primary"`, `"secondary"`, `"link"` (deprecated) |

### Size and Styling

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `size` | String | `"default"` | Size variant: `"small"`, `"large"`, `"fixed"` |
| `classes` | String | - | Additional CSS classes |
| `shopify_attributes` | String | - | Custom Shopify attributes |

### Icon Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `icon_only` | Boolean | `false` | Whether button contains only an icon |
| `icon_name` | String | - | Icon name for icon rendering |
| `icon_position` | String | `"left"` | Icon position: `"left"`, `"right"` |
| `show_icon` | Boolean | `true` | Whether to show the icon |
| `external` | Boolean | `false` | Whether link opens in new tab |

## Variant System Architecture

### Legacy Variants

Legacy variants use the `style` parameter and generate classes like `.button--primary` and `.button--secondary`. They require a `btn-fill` element for hover animations and integrate with JavaScript classes like `theme.HoverButton`.

**CSS Classes:**
- `.button--primary`
- `.button--secondary`

**Structure:**
```html
<a class="button button--primary" href="#">
  <span class="btn-fill"></span>
  Button Text
</a>
```

### Modern Variants

Modern variants use the `variant` parameter and provide pure CSS styling without JavaScript dependencies. They support more options and better performance.

**Available Variants:**
- `primary` - Main call-to-action
- `secondary` - Secondary actions
- `success` - Positive actions (Add to Cart)
- `danger` - Destructive actions (Delete)
- `ghost` - Subtle, transparent buttons
- `outline` - Bordered buttons
- `text-link` - Text-only links with underline animation
- `icon` - Circular icon buttons

**CSS Classes:**
- `.button--variant-primary`
- `.button--variant-secondary`
- etc.

### Variant Resolution Logic

The component uses this priority order to determine the variant:

```liquid
{%- if icon_only -%}
  {%- assign resolved_variant = 'icon' -%}
{%- elsif variant -%}
  {%- assign resolved_variant = variant -%}
{%- elsif style -%}
  {%- assign resolved_variant = style -%}
{%- else -%}
  {%- assign resolved_variant = 'primary' -%}
{%- endif -%}
```

## CSS Class Mapping Reference

### Base Button Class

```css
.button {
  /* Base styles from theme.css lines 3132-3155 */
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--button-gap);
  padding: var(--button-padding-y) var(--button-padding-x);
  border-radius: var(--button-border-radius);
  font-size: var(--button-font-size);
  font-weight: var(--button-font-weight);
  line-height: var(--button-line-height);
  text-decoration: none;
  transition: var(--button-transition);
  cursor: pointer;
}
```

### Variant Classes

| Variant | CSS Class | Description |
|---------|-----------|-------------|
| primary | `.button--variant-primary` | Solid primary button |
| secondary | `.button--variant-secondary` | Solid secondary button |
| success | `.button--variant-success` | Green success button |
| danger | `.button--variant-danger` | Red danger button |
| ghost | `.button--variant-ghost` | Transparent with colored text |
| outline | `.button--variant-outline` | Transparent with border |
| text-link | `.button--variant-text-link` | Text with animated underline |
| icon | `.button--variant-icon` | Circular icon button |

### Size Modifiers

| Size | CSS Class | Padding Values |
|------|-----------|---------------|
| small | `.button--small` | Reduced padding |
| large | `.button--large` | Increased padding |
| fixed | `.button--fixed` | Fixed dimensions |

### Icon Classes

| Class | Purpose |
|-------|---------|
| `.icon-with-text` | Button with both icon and text |
| `.btn-text` | Text portion of button |
| `.button-icon` | Icon portion of button |

## Theme Settings Integration

### Settings Schema Structure

The button settings are defined in `config/settings_schema.json` lines 620-905, including:

- `button_variant_style`: Toggle between "classic" and "modern" modes
- `buttons_hover`: Hover effect settings
- Border and shadow configurations
- Per-variant color settings

### Color Settings Table

| Variant | Background | Text | Hover |
|---------|------------|------|-------|
| Primary | `--color-button-primary-bg` | `--color-button-primary-text` | `--color-button-primary-hover` |
| Secondary | `--color-button-secondary-bg` | `--color-button-secondary-text` | `--color-button-secondary-hover` |
| Success | `--color-button-success-bg` | `--color-button-success-text` | `--color-button-success-hover` |
| Danger | `--color-button-danger-bg` | `--color-button-danger-text` | `--color-button-danger-hover` |
| Ghost | `--color-button-ghost-bg` | `--color-button-ghost-text` | `--color-button-ghost-hover` |
| Outline | `--color-button-outline-bg` | `--color-button-outline-text` | `--color-button-outline-hover` |
| Text-link | `--color-button-text-link-bg` | `--color-button-text-link-text` | `--color-button-text-link-hover` |
| Icon | `--color-button-icon-bg` | `--color-button-icon-text` | `--color-button-icon-hover` |
| Disabled | `--color-button-disabled-bg` | `--color-button-disabled-text` | N/A |

### CSS Variables Flow

Settings flow from `config/settings_schema.json` → `snippets/css-variables.liquid` (lines 87-115) → CSS custom properties → button styles.

## Color Token System

### Two-Tier Architecture

**Tier 1:** Theme settings map to Liquid variables in `css-variables.liquid`

**Tier 2:** Liquid variables output as CSS custom properties

### Fallback System

The system uses fallback tokens defined in `:root` (theme.css lines 14-59) to ensure buttons work even when modern variants are disabled.

## Button Rendering Logic

### Three Rendering Paths

1. **Link-style buttons** (`style='link'`): Simple anchor tags without button styling
2. **Button elements** (`element_type='button'`): Native `<button>` elements for forms
3. **Link elements styled as buttons** (default): Anchor tags with button classes

### btn-fill Element

The `btn-fill` element is only included for legacy primary/secondary variants and is used by JavaScript hover animations.

```liquid
{%- assign needs_btn_fill = style == 'primary' or style == 'secondary' -%}
```

## Special Variant Behaviors

### Ghost Variant
- Transparent background
- Colored text
- 10% opacity hover
- Lift animation

### Outline Variant
- Transparent background
- Border styling
- 10% opacity hover
- Lift animation

### Text-link Variant
- No border-radius
- Animated underline using `::after` pseudo-element
- Width transition from 0% to 100%

```css
.button--variant-text-link::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 0;
  height: 1px;
  background-color: currentColor;
  transition: width var(--animation-primary);
}
```

### Icon Variant
- Circular shape (border-radius: 50%)
- Fixed 40×40px dimensions
- 20×20px icon size

## Usage Examples

### Basic Button with Modern Variant
```liquid
{% render 'button', label: 'Add to Cart', variant: 'success' %}
```

### Form Submit Button
```liquid
{% render 'button', label: 'Submit', element_type: 'submit', button_type: 'submit', form_id: 'contact-form' %}
```

### Icon-only Button
```liquid
{% render 'button', icon_only: true, icon_name: 'close', variant: 'icon' %}
```

### Legacy Style Button
```liquid
{% render 'button', label: 'Button', style: 'primary' %}
```

### Button with Custom Attributes
```liquid
{% render 'button', label: 'Quick View', shopify_attributes: 'data-product-id="{{ product.id }}" data-modal-trigger' %}
```

## Accessibility Features

### Focus Management
- `:focus-visible` styles for keyboard navigation
- Proper contrast ratios
- Clear focus indicators

### State Handling
- Disabled state styling and cursor behavior
- Active state feedback
- Reduced motion support

### Semantic HTML
- Proper button/link elements
- ARIA attributes where needed
- Screen reader compatibility

## Migration Guide

### Enabling Modern Variants
1. Go to Theme Settings
2. Find "Button variant style"
3. Change from "Classic" to "Modern"

### Legacy to Modern Conversion

**Before:**
```liquid
{% render 'button', label: 'Button', style: 'primary' %}
```

**After:**
```liquid
{% render 'button', label: 'Button', variant: 'primary' %}
```

### Key Differences
- **No JavaScript animations:** All hover effects are now pure CSS
- More variant options available (8 modern variants vs 2 legacy)
- Better performance and accessibility
- Simplified implementation

## Common Patterns

### Form Submit Buttons
```liquid
{% render 'button', label: 'Subscribe', element_type: 'submit', variant: 'primary' %}
```

### Quick View Buttons
```liquid
{% render 'button', label: 'Quick View', variant: 'outline', shopify_attributes: 'data-quick-view' %}
```

### Delete Buttons
```liquid
{% render 'button', label: 'Delete', variant: 'danger', size: 'small' %}
```

### Icon-only Close Buttons
```liquid
{% render 'button', icon_only: true, icon_name: 'close', variant: 'icon' %}
```

## Troubleshooting

### Hover Animations Not Working
**JavaScript hover animations have been removed.** Modern variants use pure CSS for hover effects. Ensure you're using modern variants (`variant` parameter) instead of legacy styles (`style` parameter).

### Modern Variants Not Applying
- Verify `button_variant_style` is set to "modern"
- Check theme settings are saved

### btn-fill Element Issues
- `btn-fill` is only for legacy variants
- Modern variants don't use this element

### Color Customization
- Modify settings in theme customizer
- Or override CSS custom properties

## Technical Reference

### CSS Custom Properties
- `--color-button-{variant}-{property}`
- `--button-padding-x/y`
- `--button-border-radius`
- `--button-transition`

### CSS Classes
- `.button` (base)
- `.button--variant-*` (modern variants)
- `.button--*` (legacy variants)
- `.button--small/large/fixed` (sizes)

### Parameter Validation
- `variant`: Must be one of the 8 modern variants
- `style`: Must be "primary", "secondary", or "link"
- `size`: Must be "small", "large", "fixed", or default

### Browser Compatibility
- Modern CSS features require recent browsers
- Fallback to legacy variants for older browsers
- Graceful degradation for unsupported features