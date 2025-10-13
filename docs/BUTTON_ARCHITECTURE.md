# Button Component Architecture

## System Overview

**Note: JavaScript animation classes (HoverButton, MagnetButton, RevealButton) have been removed from this theme.** The button system now uses only modern CSS variants for all hover and click effects. This provides better performance, accessibility, and maintainability.

The button component system is built on a modular architecture that separates concerns across multiple files:

- **`snippets/button.liquid`**: Component logic and HTML generation
- **`assets/theme.css`**: Styling rules and CSS custom properties
- **`snippets/css-variables.liquid`**: Settings-to-CSS mapping
- **`config/settings_schema.json`**: Merchant configuration interface

### Component Diagram

```
Theme Settings (settings_schema.json)
        ↓
CSS Variables (css-variables.liquid)
        ↓
CSS Custom Properties
        ↓
Button Component (button.liquid)
        ↓
Rendered HTML + CSS Classes
```

### Data Flow

1. Merchant configures settings in theme customizer
2. Settings saved to `settings_schema.json` structure
3. `css-variables.liquid` maps settings to CSS custom properties
4. `button.liquid` reads parameters and generates appropriate classes
5. CSS applies styling based on generated classes and custom properties

## Component Lifecycle

### 1. Parameter Ingestion
Component accepts parameters and assigns defaults:

```liquid
{%- assign label = label | default: 'Button' -%}
{%- assign variant = variant | default: 'primary' -%}
```

### 2. Variant Resolution
Priority-based variant determination:

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

### 3. Class Name Construction
Dynamic class generation based on parameters:

```liquid
{%- assign button_classes = 'button' -%}
{%- if resolved_variant == 'icon' -%}
  {%- assign button_classes = button_classes | append: ' button--variant-icon' -%}
{%- else -%}
  {%- assign button_classes = button_classes | append: ' button--variant-' | append: resolved_variant -%}
{%- endif -%}
```

### 4. Element Type Determination
HTML element selection based on use case:

```liquid
{%- if element_type == 'button' or element_type == 'submit' -%}
  {%- assign tag_name = 'button' -%}
{%- else -%}
  {%- assign tag_name = 'a' -%}
{%- endif -%}
```

### 5. HTML Generation
Conditional rendering based on element type and parameters.

## Variant System Design

### Design Decision: Dual-Mode Architecture

**Why both legacy and modern variants exist:**
- **Backward Compatibility**: Existing themes using `style` parameter continue working
- **Progressive Enhancement**: New themes can use modern variants for better performance
- **Migration Path**: Clear upgrade path from legacy to modern

### Legacy System
- Uses `btn-fill` element for hover animations
- Requires JavaScript (`theme.HoverButton` class)
- Limited to primary/secondary styles
- Structure: `<a><span class="btn-fill"></span>Text</a>`

### Modern System
- Pure CSS approach with no JavaScript dependency
- 8 variant options vs 2 legacy styles
- Better performance and maintainability
- Structure: `<a>Text</a>` with CSS classes

### Coexistence Strategy
- `icon_only` parameter forces icon variant
- `variant` parameter takes precedence over `style`
- Fallback to primary if no variant specified
- CSS variables conditionally output based on `button_variant_style` setting

## CSS Architecture

### Base Styles
The `.button` class provides foundation styling:

```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--button-gap);
  padding: var(--button-padding-y) var(--button-padding-x);
  /* ... additional base styles */
}
```

### Variant Styles
Additive approach using BEM-style modifiers:

```css
.button--variant-primary {
  background-color: var(--color-button-primary-bg);
  color: var(--color-button-primary-text);
}
```

### Modifier Styles
Size and state modifiers are additive:

```css
.button--small {
  padding: var(--button-padding-y-small) var(--button-padding-x-small);
}
```

### Pseudo-elements
Used for enhanced styling effects:

```css
.button--variant-text-link::after {
  /* Animated underline */
}
```

### CSS Custom Properties
Enable runtime theming without CSS recompilation:

```css
:root {
  --color-button-primary-bg: #000;
  --color-button-primary-text: #fff;
}
```

### Specificity Management
- Base class: `.button` (0,0,1,0)
- Variants: `.button--variant-*` (0,0,2,0)
- States: `.button:hover` (0,0,1,1)
- Avoids conflicts through careful naming

## Token System Design

### Token Hierarchy

```
Settings Schema → Liquid Variables → CSS Custom Properties → CSS Rules
```

### Naming Convention

```
--color-button-{variant}-{property}
```

Examples:
- `--color-button-primary-bg`
- `--color-button-ghost-hover`
- `--color-button-outline-text`

### Fallback Chain

1. **Primary token**: Variant-specific setting
2. **Default token**: `--color-button-{variant}-{property}-default`
3. **Base token**: `--color-base-button-{property}`

### Conditional Output

Modern tokens only output when `button_variant_style == 'modern'`:

```liquid
{%- if settings.button_variant_style == 'modern' -%}
  --color-button-primary-bg: {{ settings.color_button_primary_bg }};
{%- endif -%}
```

## State Management

### Default State
Base styling applied through `.button` class.

### Hover State
- CSS `:hover` for modern variants
- JavaScript enhancement for legacy variants via `theme.HoverButton`

### Focus State
`:focus-visible` for keyboard navigation accessibility.

### Active State
`:active` for tactile feedback on touch devices.

### Disabled State
`[disabled]` attribute and `[aria-disabled]` for accessibility.

## Responsive Design

### Fluid Padding
Uses `clamp()` for responsive padding:

```css
--button-padding-x: clamp(1rem, 4vw, 2rem);
```

### Icon Adjustments
Mobile-specific icon sizing:

```css
@media (max-width: 768px) {
  .button-icon {
    width: 16px;
    height: 16px;
  }
}
```

### Touch Considerations
Hover states disabled on touch devices via media queries.

## Performance Considerations

### Modern Variants Advantage
- No JavaScript required for hover effects
- Reduced DOM manipulation
- Better paint performance

### CSS Optimizations
- `will-change` property for animated elements
- Containment for better rendering performance

### Reduced Motion Support
Respects `prefers-reduced-motion` for accessibility.

## Extensibility Points

### Adding New Variants
1. Update `button.liquid` variant resolution logic
2. Add CSS rules in `theme.css`
3. Add settings in `settings_schema.json`
4. Add CSS variables in `css-variables.liquid`

### Custom Classes
`classes` parameter allows additional styling.

### Custom Attributes
`shopify_attributes` parameter for data attributes.

### CSS Custom Properties
Override defaults for custom theming.

## Integration Points

### Icon System
Uses `snippets/icon.liquid` for icon rendering.

### Animation System
**JavaScript animation classes (HoverButton, MagnetButton, RevealButton) have been removed.** Buttons now use pure CSS animations for hover and active states, providing better performance and accessibility.

### Form System
`form_id` parameter for form association.

### Theme Settings
Reads from global `settings` object.

### Accessibility System
Uses focus color tokens and semantic HTML.

## Code Organization

### File Responsibilities

- **`snippets/button.liquid`** (166 lines): Logic and HTML generation
- **`assets/theme.css`** (lines 3100-3599): Styling rules
- **`snippets/css-variables.liquid`** (lines 79-115): Settings mapping
- **`config/settings_schema.json`** (lines 620-905): Configuration UI

### Separation Benefits

- **Maintainability**: Changes isolated to relevant files
- **Performance**: CSS can be cached separately
- **Reusability**: Component logic separate from styling
- **Testing**: Each layer can be tested independently

## Testing Considerations

### Variant Combinations
Test all variant × size × state combinations.

### Mode Switching
Verify legacy and modern modes both work.

### Accessibility
- Keyboard navigation
- Screen reader compatibility
- Focus management

### Responsive Behavior
Test at different viewport sizes.

### Theme Settings
Verify setting changes apply correctly.

## Future Enhancements

### Deprecation Path
Phase out legacy variants over time.

### Additional Variants
More semantic variants (warning, info, etc.).

### Animation Customization
Configurable animation durations and easing.

### Size Expansion
More size options (extra-small, extra-large).

### Icon Flexibility
More icon position options (top, bottom).