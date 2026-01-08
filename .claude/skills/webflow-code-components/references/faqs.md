# FAQs and Troubleshooting

Frequently asked questions about code components.

## Getting started

**How do I create my first code component?**

1. Install dependencies:
   ```bash
   npm i --save-dev @webflow/webflow-cli @webflow/react @webflow/data-types
   ```
2. [Declare a code component](./component-declaration.md).
3. Share your library:
   ```bash
   npx webflow library share
   ```

**Which frameworks can I use?**

Code components support React with TypeScript. Most React libraries work, but CSS-in-JS tools require Shadow DOM setup. See [Frameworks and Libraries](./frameworks-and-libraries.md).

**How many components can I share?**

There’s no strict limit, but keep bundle size under 50MB for best performance.

## Development and styling

**Why can’t I see my component styles?**

Components run in Shadow DOM. Site classes don’t apply. Import styles in your `.webflow.tsx` file or use a globals file.

**How do I use site variables?**

Reference CSS variables:

```css
.my-component {
  color: var(--background-primary, #000);
}
```

## Imports and updates

**How do I update a component?**

Update your code and re-run `npx webflow library share`. The entire library is redeployed.

**Can I test locally?**

Yes. Use the bundle command with `--dev`:

```bash
npx webflow library bundle --public-path http://localhost:4000/ --dev
```

## Troubleshooting

**My component isn’t rendering**

- Check for build errors in the CLI output.
- Ensure required dependencies are installed.
- Verify the component has a single root element.
- Keep bundle size under 50MB.

**Library import fails**

- Verify your workspace token is valid.
- Check for bundling errors or network issues.
- Run with `--verbose` for more details.
