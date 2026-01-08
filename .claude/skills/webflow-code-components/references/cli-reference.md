# Webflow CLI

Learn about the Webflow CLI and DevLink workflows.

## Installation

```bash
npm i --save-dev @webflow/webflow-cli
```

## Commands

### Import (share)

Share your library to your Workspace:

```bash
npx webflow library share
```

This command will:

- Authorize your workspace (prompt if no token is found)
- Bundle your library
- Ask you to confirm components to share
- Upload your library to your workspace

#### Options

| Option | Description | Default |
| --- | --- | --- |
| `--manifest` | Path to `webflow.json` | Scans current directory |
| `--api-token` | Workspace API token override | Uses `WEBFLOW_WORKSPACE_API_TOKEN` |
| `--no-input` | Disable prompts (CI/CD) | Off |
| `--verbose` | Verbose logging | Off |
| `--dev` | Development bundle (no minify/sourcemaps) | Off |

### Bundle

Bundle your library locally for debugging:

```bash
npx webflow library bundle --public-path http://localhost:4000/
```

#### Options

| Option | Description |
| --- | --- |
| `--public-path` | Required URL where you will serve the bundle |
| `--force` | Finish bundling even with warnings |
| `--dev` | Development bundle (no minify/sourcemaps) |
| `--debug-bundler` | Print the final webpack configuration |

### Log

View logs for your last library import:

```bash
npx webflow library log
```

## CI/CD workflows

Use `--no-input` to avoid interactive prompts in automated pipelines:

```bash
npx webflow library share --no-input
```

> Implement change detection so you don’t unintentionally remove components by sharing unchanged libraries.

## Troubleshooting

### Authentication

- Ensure `.env` includes `WEBFLOW_WORKSPACE_API_TOKEN`.
- Use `--verbose` for more details.
- Try `--api-token` to pass a token directly.

### Prompts for component removal

If the CLI warns about removing components, check that your `.webflow.tsx` filename and component name have not changed. Renaming a definition file creates a new component and removes the old one.
