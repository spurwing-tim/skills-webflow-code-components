# Styling Components

Learn how to style code components for use in Webflow.

## How Shadow DOM affects styling

Code components render inside Shadow DOM, which creates an isolated styling boundary. This means:

- Component styles won't affect the page
- Page styles won't affect your component
- You must explicitly connect to external styles

## Adding styles to your code components

Import styles directly in the `.webflow.tsx` definition file:

```tsx
import { declareComponent } from '@webflow/react';
import { props } from '@webflow/data-types';
import { Button } from './Button';
import './Button.module.css';

export default declareComponent(Button, {
  name: 'Button',
  props: {
    text: props.Text({ name: 'Text', defaultValue: 'Click me' }),
  },
});
```

## Global styles

To apply shared styles across all components, import them in a globals file and reference it in `webflow.json`:

```ts
// globals.ts
import './globals.css';
```

```json
{
  "library": {
    "globals": "./src/globals.ts"
  }
}
```

## CSS capabilities in Shadow DOM

| Feature | Works in Shadow DOM | How to use |
| --- | --- | --- |
| Site variables | ✅ Yes | `var(--background-primary, fallback)` |
| Inherited properties | ✅ Yes | `font-family: inherit` |
| Tag selectors | ✅ Yes | Enable `applyTagSelectors: true` |
| Site classes | ❌ No | Use component-specific classes |

### Site variables

Reference Webflow Variables with CSS custom properties:

```css
.button {
  color: var(--background-primary, #000);
}
```

### Inherited properties

Use `inherit` to pull values across the Shadow DOM boundary:

```css
.button {
  font-family: inherit;
}
```

### Tag selectors

Enable `applyTagSelectors` in your component definition to inherit tag selector styles:

```tsx
export default declareComponent(Button, {
  name: 'Button',
  options: {
    applyTagSelectors: true,
  },
});
```

## Advanced styling

For Tailwind, CSS-in-JS, or component libraries, see the [Frameworks and Libraries](./frameworks-and-libraries.md) guide.
