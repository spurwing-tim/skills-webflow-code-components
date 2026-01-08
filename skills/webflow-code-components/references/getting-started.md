# Getting Started

Set up your React project for Webflow code components and publish your first component with DevLink.

## Before you start

Make sure you have:

- Node.js 20+ and npm 10+
- A Webflow account with either:
  - a Workspace on a Freelancer, Core, Growth, Agency, or Enterprise plan, or
  - a Webflow site on a CMS, Business, or Enterprise plan
- A Webflow site where you can test components
- Familiarity with React and TypeScript

## 1. Set up your development environment

### Create or open a React project

If you need a new project:

```bash
npx create-react-app code-components
cd code-components
```

### Install the Webflow CLI and packages

```bash
npm i --save-dev @webflow/webflow-cli @webflow/data-types @webflow/react
```

## 2. Configure `webflow.json`

Create a `webflow.json` file in your project root:

```json
{
  "library": {
    "name": "<Your Library Name>",
    "components": ["./src/**/*.webflow.@(js|jsx|mjs|ts|tsx)"]
  }
}
```

This file tells DevLink which component definition files to include. For optional settings like `globals` and `bundleConfig`, see the [installation guide](./installation.md).

## 3. Add a React component

Create a simple component, for example:

```tsx
// src/Badge.tsx
import * as React from "react";

interface BadgeProps {
  text: string;
  variant: "Light" | "Dark";
}

export const Badge = ({ text, variant }: BadgeProps) => (
  <span
    style={{
      backgroundColor: variant === "Light" ? "#eee" : "#000",
      borderRadius: "1em",
      color: variant === "Light" ? "#000" : "#fff",
      display: "inline-block",
      fontSize: "14px",
      lineHeight: 2,
      padding: "0 1em",
    }}
  >
    {text}
  </span>
);
```

## 4. Define the Webflow code component

Create a `.webflow.tsx` file to map the React component to Webflow:

```tsx
// src/Badge.webflow.tsx
import { Badge } from "./Badge";
import { props } from "@webflow/data-types";
import { declareComponent } from "@webflow/react";

export default declareComponent(Badge, {
  name: "Badge",
  description: "A badge with variants",
  group: "Info",
  props: {
    text: props.Text({
      name: "Text",
      defaultValue: "Hello World",
    }),
    variant: props.Variant({
      name: "Variant",
      options: ["Light", "Dark"],
      defaultValue: "Light",
    }),
  },
});
```

For more configuration details, see [Define a code component](./component-declaration.md).

## 5. Share your library to Webflow

Run the share command:

```bash
npx webflow library share
```

The CLI will:

- Authorize your workspace (or use `.env` if already configured)
- Bundle the library
- Ask you to confirm the components to share
- Upload the library to your workspace

## 6. Install and use the library in Webflow

1. Open a Webflow site in your workspace.
2. Open the **Libraries** panel (press **L**).
3. Find your library and click **Install**.
4. Open the **Components** panel (press **⇧C**).
5. Drag your component onto the canvas and edit props in the right panel.

## Next steps

- **[Installation](./installation.md)** - Configure authentication and globals
- **[Prop Types](./prop-types.md)** - Explore all available prop types
- **[Bundling and Import](./bundling-and-import.md)** - Advanced bundling options
- **[Styling Components](./styling.md)** - Shadow DOM styling guidance
