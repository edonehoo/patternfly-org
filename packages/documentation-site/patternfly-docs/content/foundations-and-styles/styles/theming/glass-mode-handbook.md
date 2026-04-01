---
id: Theming
section: foundations-and-styles
source: glass-mode-handbook
---

# Glass mode developer handbook 

## What is glass mode?

Glass mode is a contrast mode option that's enabled in our Felt theme by default and can be manually enabled in our Default theme. Glass adds transparency, blurring, and depth to the UI to create a more dynamic visual effect where brand-approved background images and layered UI elements subtly show through. 

When turned on, glass applies to the following components, including their uses within our extensions:
- Card
- Drawer
- Login page 
- Masthead
- Navigation (docked)
- Page
- Panel

### Background images

The glass effect is most visible when placed over a background image. Background images should be selected or designed for each Red Hat product in collaboration with the Brand team. To retain readability and ensure proper contrast ratios are met, images shouldn't contain high levels of detail or extreme contrast. 

Text must never be placed directly on a background image&mdash;it should be placed within a container that has a background color or glass effect. Titles or headings with stronger font weights can be placed directly on background images only if they pass specific brand and contrast requirements.

### Opacity

The default opacity values used in our components have been tested for accessibility, legibility, and visual appeal.
- Felt light typically utilizes a 40% opacity to maintain AAA contrast for standard text.
- Felt dark utilizes a 60% opacity to provide better reliability for the dark color scheme.

If a product team chooses to override these token values, they are responsible for conducting their own accessibility evaluation to ensure WCAG compliance.

## Enabling glass mode

Glass mode is designed to work across light and dark color schemes and is enabled by default in our Felt theme. To manually enable glass in our Default theme, add the class `.pf-v6-theme-glass` to your application’s `<html>` tag. When implementing glass with our Default theme, it's important to ensure that glass does not harm the overall accessibility or usability of your product.

### Technical constraints

There are a few technical constraints to abide by when using glass in your product.

- **No glass-on-glass layering:** Never layer glass-enabled containers. Doing so can cause significant performance and accessibility problems. For example, child objects inside a blurred parent container can become illegibly blurred themselves. To mitigate this, we have intentionally adjusted the opacity of our background color design tokens to simulate depth without introducing extra blur.
- **High contrast precedence:** If high-contrast mode is enabled, all glass effects must be automatically disabled to prioritize functional accessibility.
- **User controls and preferences:** Because glass mode introduces legibility risks, any product utilizing glass must also let users swap between default contrast and high contrast modes via a theme switcher or preferences menu. Products should also respect the OS-level `prefers-reduced-transparency` media query, disabling glass or replacing it with a solid high-opacity background to accommodate users appropriately. 

## Glass design tokens 

The following tokens are used together to create the glass effect.

| Token | Value (in Felt theme) | Usage |
| --- | --- | --- |
| `-pf-t--global--background--color--glass--primary--default` | Light mode value: #FFFFFF (60% opacity) <br />  Dark mode value: #292929 at 50% opacity | Base fill for glass containers. |
| `-pf-t--global--background--filter--glass--default` | Light mode value: #C7C7C7 (25% opacity) <br />  Dark mode value: #C7C7C7 (15% opacity) | Overlay tint used to enhance blur visibility. |
| `-pf-t--global--border--color--glass--default` | Light mode value: #FFFFFF (60% opacity) <br />  Dark mode value: #292929 (50% opacity) | Boundary highlight for glass surfaces. |
| `-pf-t--global--border--radius--glass--default` |  #D6D6D6 with 16px radius | Default rounded border for glass element. |
| `-pf-t--global--background--color--sticky--default` | Light mode value: #FFFFFF (100% opacity) <br />  Dark mode value: #292929 (100% opacity) | Solid background for sticky elements, like headers. |
| `-pf-t--global--box-shadow--medium` | #292929 (15% opacity) | Shadow that signifies elevation. |

## Best practices

To ensure a successful implementation, follow these best practices:
- Ensure all text meets a 4.5:1 (AA) contrast ratio.
- Ensure that high contrast mode overrides any use of glass to maintain accessibility requirements. 
- Always verify glass components against both light and dark background variations to catch contrast failures early.