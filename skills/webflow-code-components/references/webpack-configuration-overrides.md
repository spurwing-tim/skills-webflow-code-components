# Webpack Configuration Overrides

Customize your webpack configuration for advanced code component setups.

## Overview

The CLI uses a built-in webpack configuration optimized for Webflow. You can provide an override config that merges with the default configuration.

### Inspect the default config

```bash
npx webflow library bundle --debug-bundler
```

## Setup

1. Add `bundleConfig` in `webflow.json`:
   ```json
   {
     "library": {
       "name": "My Library",
       "components": ["./src/components/**/*.webflow.{js,ts,tsx}"],
       "bundleConfig": "./webpack.override.js"
     }
   }
   ```
2. Create a CommonJS override file:
   ```js
   module.exports = {
     resolve: {
       extensions: [".js", ".ts", ".tsx", ".json"],
     },
   };
   ```

## Configuration rules

### Blocked properties

The following properties are ignored for security and compatibility:

- `entry`
- `output`
- `target`

### Module rules

Provide a function to mutate the existing rules:

```js
module.exports = {
  module: {
    rules: (currentRules) => {
      return currentRules.map((rule) => rule);
    },
  },
};
```

### Plugins

Plugins are merged with the default configuration and de-duplicated where necessary (e.g. `ModuleFederationPlugin`, `MiniCssExtractPlugin`).

## Examples

### Add a loader (SCSS)

```js
module.exports = {
  module: {
    rules: (currentRules) => {
      const currentCSSRule = currentRules.find(
        (rule) =>
          rule.test instanceof RegExp &&
          rule.test.test("test.css") &&
          Array.isArray(rule.use)
      );
      return [
        ...currentRules,
        {
          test: /\.scss$/,
          use: [...currentCSSRule.use, "sass-loader"],
        },
      ];
    },
  },
};
```

### Add a new rule

```js
module.exports = {
  module: {
    rules: (currentRules) => [
      ...currentRules,
      {
        test: /\.md$/,
        use: ["markdown-loader"],
      },
    ],
  },
};
```

### Extend an existing loader

```js
module.exports = {
  module: {
    rules: (currentRules) =>
      currentRules.map((rule) => {
        if (
          rule.test instanceof RegExp &&
          rule.test.test("test.css") &&
          Array.isArray(rule.use)
        ) {
          for (const [index, loader] of rule.use.entries()) {
            if (typeof loader === "object" && loader?.ident === "css-loader") {
              const options = typeof loader.options === "object" ? loader.options : {};
              rule.use[index] = {
                ...loader,
                options: {
                  ...options,
                  modules: {
                    exportLocalsConvention: "as-is",
                    namedExport: false,
                  },
                },
              };
            }
          }
        }
        return rule;
      }),
  },
};
```
