# Installation

Configure DevLink in a React project for code component imports.

## Setup requirements

### Install the Webflow CLI

```bash
npm i --save-dev @webflow/webflow-cli @webflow/data-types @webflow/react
```

**What you get:**

- `@webflow/webflow-cli` - CLI used to publish components to Webflow
- `@webflow/data-types` - Prop type definitions
- `@webflow/react` - React utilities for code components

### Configure `webflow.json`

Create or update `webflow.json` in your project root:

```json
{
  "library": {
    "name": "<Your Library Name>",
    "components": ["./src/**/*.webflow.@(js|jsx|mjs|ts|tsx)"],
    "bundleConfig": "./webpack.webflow.js",
    "globals": "./src/globals.ts"
  }
}
```

| Field | Description | Required |
| --- | --- | --- |
| `library.name` | Library name shown in Webflow | Yes |
| `library.components` | Glob pattern for code component definition files | Yes |
| `library.bundleConfig` | Path to custom webpack overrides | No |
| `library.globals` | Path to a globals/decorators file | No |

### Authentication

When you run `npx webflow library share`, the CLI will prompt you to authenticate and save a workspace token to `.env`. For manual auth:

```bash
npx webflow library share --api-token <your-api-token>
```

**Workspace API token:**

1. Open your Workspace and go to **Apps & Integrations** → **Manage**.
2. Scroll to **Workspace API Access** and generate a token.
3. Add it to `.env` as `WEBFLOW_WORKSPACE_API_TOKEN=your-token`.

> You must be a Workspace Admin to create a Workspace API token.

## Next steps

- **[Define a code component](./component-declaration.md)**
- **[Webflow CLI reference](./cli-reference.md)**
