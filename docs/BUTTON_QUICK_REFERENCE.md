# Button Component Quick Reference

## Quick Start

```liquid
{% render 'button', label: 'Click Me' %}
```

See [BUTTON_COMPONENT_SYSTEM.md](BUTTON_COMPONENT_SYSTEM.md) for complete documentation.

## Parameter Cheat Sheet

| Parameter | Type | Common Values | Description |
|-----------|------|---------------|-------------|
| `label` | String | Any text | **Required.** Button text |
| `variant` | String | `primary`, `secondary`, `success`, `danger`, `ghost`, `outline`, `text-link`, `icon` | Button style |
| `size` | String | `small`, `large`, `fixed` | Button size |
| `element_type` | String | `link`, `button`, `submit` | HTML element type |
| `icon_name` | String | `cart`, `heart`, `close` | Icon to display |
| `icon_only` | Boolean | `true`, `false` | Icon-only button |
| `link` | String | `/collections/all` | Link URL |
| `classes` | String | `my-custom-class` | Additional CSS classes |
| `disabled` | Boolean | `true`, `false` | Disable button |
| `shopify_attributes` | String | `data-modal-trigger` | Custom attributes |

*Required parameters in **bold**. Most common parameters highlighted.

## Variant Quick Reference

| Variant | CSS Class | Use Case | Example |
|---------|-----------|----------|---------|
| `primary` | `.button--variant-primary` | Main CTA | Add to Cart |
| `secondary` | `.button--variant-secondary` | Secondary action | View Details |
| `success` | `.button--variant-success` | Positive action | ✅ Confirm |
| `danger` | `.button--variant-danger` | Destructive action | 🗑️ Delete |
| `ghost` | `.button--variant-ghost` | Subtle action | ⋯ More Options |
| `outline` | `.button--variant-outline` | Alternative CTA | Outline Button |
| `text-link` | `.button--variant-text-link` | Inline link | Read More → |
| `icon` | `.button--variant-icon` | Icon action | 🔍 Search |

## Common Recipes

### Primary CTA Button
```liquid
{% render 'button', label: 'Add to Cart', variant: 'primary' %}
```

### Secondary Button
```liquid
{% render 'button', label: 'Learn More', variant: 'secondary' %}
```

### Success Button (Add to Cart)
```liquid
{% render 'button', label: 'Add to Cart', variant: 'success' %}
```

### Danger Button (Delete)
```liquid
{% render 'button', label: 'Delete', variant: 'danger', size: 'small' %}
```

### Ghost Button
```liquid
{% render 'button', label: 'Cancel', variant: 'ghost' %}
```

### Outline Button
```liquid
{% render 'button', label: 'View Details', variant: 'outline' %}
```

### Text Link Button
```liquid
{% render 'button', label: 'Read More', variant: 'text-link' %}
```

### Icon-only Button
```liquid
{% render 'button', icon_only: true, icon_name: 'close', variant: 'icon' %}
```

### Submit Button
```liquid
{% render 'button', label: 'Submit', element_type: 'submit', variant: 'primary' %}
```

## CSS Class Lookup

| Liquid Parameter | Generated CSS Class |
|------------------|---------------------|
| `variant: 'primary'` | `.button--variant-primary` |
| `variant: 'secondary'` | `.button--variant-secondary` |
| `size: 'small'` | `.button--small` |
| `size: 'large'` | `.button--large` |
| `icon_only: true` | `.button--variant-icon` |
| Legacy `style: 'primary'` | `.button--primary` |

## Settings Lookup

| Setting ID | What It Controls |
|------------|------------------|
| `button_variant_style` | Classic vs Modern variants |
| `buttons_hover` | Hover animation effects |
| `button_border_radius` | Button corner rounding |
| `button_shadow` | Button shadow effects |
| `color_button_*` | Button colors per variant |

*Find these in Theme Settings → Buttons section.

## Troubleshooting Checklist

- **Button not styled?** Check if `variant` parameter is set
- **Modern variants not working?** Verify `button_variant_style` is "modern" in theme settings
- **Hover effects missing?** Check `buttons_hover` setting is enabled
- **Icon not showing?** Ensure `icon_name` is valid and `snippets/icon.liquid` exists
- **Button disabled?** Check `disabled: false` or remove parameter
- **Wrong size?** Use `size: 'small'`, `'large'`, or `'fixed'`
- **Custom styling?** Add classes via `classes` parameter
- **Link not working?** Set `link` parameter for navigation