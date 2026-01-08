# Bundling and Import

Learn how to bundle and import your React components into Webflow.

## Import (share)

Use DevLink to share your library to a Workspace:

```bash
npx webflow library share
```

For CI/CD workflows, add `--no-input` to disable prompts:

```bash
npx webflow library share --no-input
```

> Add change detection to avoid unintentionally removing components.

## Bundling

The CLI uses Webpack to bundle components. The default configuration handles most projects, but you can extend it for:

- Custom CSS processing
- Specialized asset handling (SVG, fonts)
- Build optimizations (tree shaking, code splitting)

To override the default configuration, see [Webpack Configuration Overrides](./webpack-configuration-overrides.md).

### Bundle limits

- Maximum bundle size: **50MB**

## Debugging

### Disable minification

Use a custom webpack config to switch to development mode:

```js
// webpack.webflow.js
module.exports = {
  mode: 'development',
};
```

Make sure the override file is referenced in `webflow.json`:

```json
{
  "library": {
    "bundleConfig": "./webpack.webflow.js"
  }
}
```

### CSS Modules dot notation

The default config uses bracket notation for CSS Modules. To enable dot notation, adjust `css-loader` options in your override config:

```js
module.exports = {
  module: {
    rules: (currentRules) => {
      return currentRules.map((rule) => {
        if (
          rule.test instanceof RegExp &&
          rule.test.test('test.css') &&
          Array.isArray(rule.use)
        ) {
          for (const [index, loader] of rule.use.entries()) {
            if (typeof loader === 'object' && loader?.ident === 'css-loader') {
              const options = typeof loader.options === 'object' ? loader.options : {};
              rule.use[index] = {
                ...loader,
                options: {
                  ...options,
                  modules: {
                    exportLocalsConvention: 'as-is',
                    namedExport: false,
                  },
                },
              };
            }
          }
        }
        return rule;
      });
    },
  },
};
```

## Bundle locally

Bundle and serve locally for debugging:

```bash
npx webflow library bundle --public-path http://localhost:4000/
```

Use `--debug-bundler` to print the final webpack configuration.
