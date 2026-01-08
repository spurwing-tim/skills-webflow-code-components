# Webflow Hooks

Access Webflow-specific context and information in your React components.

## `useWebflowContext()`

Call `useWebflowContext` to get the current Webflow environment.

### Syntax

```typescript
useWebflowContext(): WebflowContext;

type WebflowContext = {
  mode: WebflowMode;
  interactive: boolean;
  locale: string | null;
};

type WebflowMode =
  | "design"
  | "build"
  | "edit"
  | "preview"
  | "component-preview"
  | "comment"
  | "analyze"
  | "publish";
```

### Returns

| Property | Type | Description |
| --- | --- | --- |
| `mode` | `WebflowMode` | Current Webflow mode |
| `interactive` | `boolean` | Whether the component is interactive |
| `locale` | `string \| null` | Current locale (ISO string) or `null` |

## When to use

Use `useWebflowContext` when you need to:

- Adapt behavior based on the current mode
- Disable interactions in design contexts
- Render locale-aware content

## Examples

### Conditional rendering based on `interactive`

```tsx
import { useWebflowContext } from '@webflow/react';
import { Accordion, AccordionSummary, AccordionDetails } from '@mui/material';

export const FAQItem = ({ question, answer }) => {
  const { interactive } = useWebflowContext();

  return (
    <Accordion defaultExpanded={!interactive}>
      <AccordionSummary>{question}</AccordionSummary>
      <AccordionDetails>{answer}</AccordionDetails>
    </Accordion>
  );
};
```

### Locale-aware content

```tsx
import { useWebflowContext } from '@webflow/react';

export const LocalizedHero = () => {
  const { locale } = useWebflowContext();

  const content = locale === 'es'
    ? { title: 'Bienvenido', cta: 'Comenzar' }
    : { title: 'Welcome', cta: 'Get started' };

  return (
    <section>
      <h1>{content.title}</h1>
      <button>{content.cta}</button>
    </section>
  );
};
```

## Best practices

- Provide fallback behavior if `locale` is `null`.
- Use `interactive` to prevent designer friction (e.g., disable drag).
- Avoid hiding core content in design modes.
