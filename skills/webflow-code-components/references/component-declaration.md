# Component Declaration

Reference for the `declareComponent` function used to create code component definitions.

## Overview

A code component definition file tells Webflow how to use your React component on the canvas. Every definition file is a `.webflow.tsx` or `.webflow.ts` file that calls `declareComponent`.

```tsx
import { declareComponent } from '@webflow/react';
import { props } from '@webflow/data-types';
import { Button } from './Button';

export default declareComponent(Button, {
  name: "Button",
  description: "A button component with a text and a style variant",
  group: "Interactive",
  props: {
    text: props.Text({
      name: "Button Text",
      defaultValue: "Click me",
    }),
    variant: props.Variant({
      name: "Style",
      options: ["primary", "secondary"],
      defaultValue: "primary",
    }),
  },
  options: {
    applyTagSelectors: true,
  },
});
```

## File structure and naming

- **File extension**: `.webflow.tsx` or `.webflow.ts`
- **Naming pattern**: `ComponentName.webflow.tsx`
- **Location**: Usually alongside your React component file

> Renaming a definition file creates a new component and removes the old one from your library. Existing instances in Webflow will break and require replacement.

## `declareComponent(Component, data)`

### Parameters

- **`Component`**: The React component to declare
- **`data`**: Configuration object with metadata, props, decorators, and options

### Data object

| Property | Type | Description |
| --- | --- | --- |
| `name` | string | Component name in the Designer (required) |
| `description?` | string | Description shown in the component panel |
| `group?` | string | Group label in the component panel |
| `props?` | object | Prop definitions for Webflow |
| `decorators?` | array | Wrapper decorators applied to the component |
| `options?` | object | Advanced configuration (`applyTagSelectors`, `ssr`) |

## Prop definitions

Use the `props` object to expose editable properties. Each prop maps to a Webflow prop type. See [Prop Types](./prop-types.md) for available options.

```tsx
props: {
  text: props.Text({ name: "Button Text", defaultValue: "Click me" }),
  variant: props.Variant({ name: "Style", options: ["primary", "secondary"] }),
}
```

## Component decorators

Decorators wrap your component with providers or utilities. Use them for themes, i18n, or CSS-in-JS integration. You can apply decorators per component or globally.

### Global decorators

Create a globals file and reference it in `webflow.json`:

```ts
// src/globals.ts
import "./globals.css";
```

```json
{
  "library": {
    "globals": "./src/globals.ts"
  }
}
```

To apply decorators globally, export a `decorators` array from the globals file:

```ts
import "./globals.css";
import { errorBoundaryDecorator } from "./ErrorBoundary";

export const decorators = [errorBoundaryDecorator];
```

## Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `applyTagSelectors` | boolean | `false` | Inherit site tag selector styles inside the Shadow DOM |
| `ssr` | boolean | `true` | Enable server-side rendering for the component |

### Tag selectors

Enable `applyTagSelectors` to inherit styles for tags like `h1`, `p`, and `button` from the Webflow site. See [Styling Components](./styling.md) for details.

### Server-side rendering (SSR)

SSR is enabled by default. Disable it for browser-only APIs or highly interactive components:

```tsx
export default declareComponent(Chart, {
  name: "Chart",
  options: {
    ssr: false,
  },
});
```

## Best practices

- Keep declaration files next to their React components.
- Provide clear names and descriptions.
- Group related props in the Designer using `group`.
- Set sensible default values so components work immediately.

## Next steps

- **[Prop Types](./prop-types.md)** - Explore all available prop types
- **[Styling](./styling.md)** - Style components in Shadow DOM
- **[Bundling and Import](./bundling-and-import.md)** - Share your library with Webflow
