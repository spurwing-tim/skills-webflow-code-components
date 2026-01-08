# Prop Types

Prop types define the configurable properties that designers can edit in Webflow. When you create a code component, you map your React props to Webflow prop types using the `props` helper.

## Basic usage

```tsx
import { props } from '@webflow/data-types';
import { declareComponent } from '@webflow/react';
import { Button } from './Button';

export default declareComponent(Button, {
  name: 'Button',
  props: {
    text: props.Text({
      name: 'Button text',
      defaultValue: 'Hello World!',
    }),
    variant: props.Variant({
      name: 'Style',
      options: ['primary', 'secondary'],
    }),
  },
});
```

## Available prop types

### Text

Single-line text input.

```tsx
props.Text({
  name: 'Title',
  group: 'Content',
  tooltip: 'Main heading text',
  defaultValue: 'Hello World',
})
```

**Returns:** `string`

> `props.String` is an alias of `props.Text`.

### Rich Text

Multi-line text with formatting.

```tsx
props.RichText({
  name: 'Body',
  group: 'Content',
  tooltip: 'Supports formatting',
})
```

**Returns:** `string`

### Text Node

Canvas-editable text (inline editing in the designer).

```tsx
props.TextNode({
  name: 'Heading',
  group: 'Content',
  defaultValue: 'Edit me',
})
```

**Returns:** `string`

### Link

Link picker with URL, target, and preload options.

```tsx
props.Link({
  name: 'Link',
  group: 'Navigation',
  tooltip: 'Configure the destination',
})
```

**Returns:**

```ts
{
  href: string;
  target?: "_self" | "_blank" | string;
  preload?: "prerender" | "prefetch" | "none" | string;
}
```

### Image

Image asset selection.

```tsx
props.Image({
  name: 'Image',
  group: 'Content',
})
```

**Returns:** `string` (image URL)

### Number

Numeric input with validation.

```tsx
props.Number({
  name: 'Size',
  group: 'Style',
  defaultValue: 16,
  min: 8,
  max: 64,
  decimals: 0,
})
```

**Returns:** `number`

### Boolean

Toggle control.

```tsx
props.Boolean({
  name: 'Show Icon',
  group: 'Style',
  defaultValue: true,
})
```

**Returns:** `boolean`

### Variant

Dropdown with predefined options.

```tsx
props.Variant({
  name: 'Style',
  options: ['primary', 'secondary', 'ghost'],
  defaultValue: 'primary',
  group: 'Style',
})
```

**Returns:** `string`

### Visibility

Show/hide controls for conditional rendering.

```tsx
props.Visibility({
  name: 'Show CTA',
  defaultValue: true,
  group: 'Content',
})
```

**Returns:** `boolean`

### Slot

Allows nesting of other components or elements.

```tsx
props.Slot({
  name: 'Content',
  group: 'Layout',
})
```

**Returns:** `ReactNode`

### ID

HTML element ID for anchor linking or accessibility.

```tsx
props.ID({
  name: 'Element ID',
  group: 'Accessibility',
})
```

**Returns:** `string`

## Prop values and wrapper components

Some prop types return objects. If your React component expects a different shape, create a wrapper component to map values.

```tsx
import { props, PropType, PropValues } from '@webflow/data-types';
import { declareComponent } from '@webflow/react';
import React from 'react';
import Button, { ButtonProps } from './Button';

type WebflowButtonProps = {
  link: PropValues[PropType.Link];
} & Omit<ButtonProps, 'href' | 'target'>;

const WebflowButton = ({ link: { href, target }, ...rest }: WebflowButtonProps) => {
  return <Button href={href} target={target} {...rest} />;
};

export default declareComponent(WebflowButton, {
  name: 'Button',
  props: {
    text: props.Text({ name: 'Text', defaultValue: 'Click me' }),
    link: props.Link({ name: 'Link' }),
  },
});
```

## Best practices

- Provide clear default values so components render immediately.
- Use `group` to organize related props in the Designer.
- Keep prop names short and descriptive.
