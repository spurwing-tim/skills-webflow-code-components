# Component Architecture

Learn how code components work internally and the architectural constraints to plan around.

## Overview

**Code components run as isolated React applications.** Each component mounts in its own Shadow DOM container with a separate React root. This sandboxed environment prevents conflicts with the main page or other components.

### Key concepts

- **Shadow DOM isolation** - Styles and DOM elements are contained.
- **Separate React roots** - No shared state or context between components.
- **Server-side rendering** - SSR provides initial HTML by default.
- **Client-side execution** - Interactivity runs in the browser.

## Shadow DOM and React roots

Each code component runs in its own Shadow DOM with a separate React root:

- Component styles do not leak to the page.
- Page styles do not override component styles.
- External styles must be explicitly imported or connected.

### Composing components with slots

When using `props.Slot()`, child components still render in their own Shadow DOM container. Parent and child components **cannot share state through React context**, even when composed with slots.

## Server-side rendering (SSR)

SSR is enabled by default. Webflow renders the component HTML on the server, then hydrates it on the client.

Disable SSR when components rely on browser-only APIs or render non-deterministic output:

```tsx
declareComponent(Component, {
  name: "Chart",
  options: { ssr: false },
});
```

### When to disable SSR

- Browser APIs (`window`, `document`, `localStorage`)
- Highly interactive or animation-driven UI
- Dynamic/personalized content
- Non-deterministic rendering (randomized output)

> React Server Components are not supported in code components.

## Communicating between components

Because each component has its own React root, use alternative patterns to share state:

- **URL parameters** with `URLSearchParams`
- **Browser storage** (`localStorage`, `sessionStorage`)
- **External state libraries** like Nano Stores

See [Component Communication](./component-communication.md) for implementation patterns.

## Performance considerations

- **Bundle size limit:** keep libraries under 50MB.
- Each component instance creates its own React root, event listeners, and state.
- Consider lazy loading heavy components.

## Next steps

- **[Component Communication](./component-communication.md)** - State sharing patterns
- **[Styling](./styling.md)** - Shadow DOM styling guidance
- **[Bundling and Import](./bundling-and-import.md)** - Build and share libraries
