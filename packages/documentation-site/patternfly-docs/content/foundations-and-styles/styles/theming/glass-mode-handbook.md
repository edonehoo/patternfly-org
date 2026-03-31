---
id: Theming
section: foundations-and-styles
source: glass-mode-handbook
---

# Glass mode developer handbook 

## What is glass mode?

Glass mode is a contrast mode option that's enabled in the Felt theme by default and can be manually enabled in the Default theme. Glass adds transparency, blurring, and depth to the UI to create a layered visual effect where brand-approved background images and layered UI elements subtly show through. 

### Background images

The glass effect is most visible when placed over a background image. Background images should be selected or designed for each Red Hat product in collaboration with the Brand team. To retain readability and ensure proper contrast ratios are met, images shouldn't contain high levels of detail or extreme contrast. 

Text must never be placed directly on a background image&mdash;it should be placed within a container that has a background color or glass effect. Titles or headings with stronger font weights may be placed directly on background images only if they pass specific brand and contrast requirements.

### Opacity

The minimum level of glass opacity can vary significantly depending on the content that needs to be read, the background images and colors used behind the lowest level glass overlay, and whether any custom CSS overrides are being used in a product space.

The default opacity values used in our components have been tested for accessibility, legibility, and visual appeal. Our research and testing revealed that a 40% to 60% opacity range is the ideal opacity range to balance aesthetics with usability. A 60% opacity is a slightly more cautious level that provides better reliability for dark themes. A 40% opacity provides a more distinct glass look and maintains AAA contrast for standard text in most light theme scenarios. 

## Enabling glass mode

Glass mode is enabled by default in the Felt theme and is designed to work across light and dark color schemes. To enable glass in the Default theme, add the class `.pf-v6-theme-glass` to your application’s `<html>` tag. It is important that you ensure that your use of glass does not impact the accessibility or usability of your product.

**Note:** Layering multiple glass-enabled containers can cause significant accessibility and performance problems. We do not support glass-on-glass overlays, such as a glass modal placed over a glass card. Components that overlay others in the UI do not have a glass effect by default.

### User controls and preferences
Because glass mode introduces legibility risks, any product utilizing glass must also let users swap between default contrast and high contrast modes via a theme switcher or preferences menu. When users select high contrast mode, it must always override and disable glass effects to ensure functional accessibility.

Products should also respect the OS-level `prefers-reduced-transparency` media query, disabling glass or replacing it with a solid high-opacity background to accommodate users appropriately. 

## Best practices

To ensure a successful implementation, follow these best practices:
- Ensure all text meets a 4.5:1 (AA) contrast ratio.
- Ensure that high contrast mode overrides any use of glass to maintain accessibility requirements. 
- Always verify glass components against both light and dark background variations to catch contrast failures early.