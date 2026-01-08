# Frameworks and Libraries

Learn how to use frameworks and libraries with code components.

Code components render inside Shadow DOM. Tools that inject styles into `document.head` need extra configuration to target the Shadow Root. Webflow provides utilities for common CSS-in-JS libraries.

## Tailwind CSS

1. Install Tailwind and PostCSS:
   ```bash
   npm install tailwindcss @tailwindcss/postcss postcss
   ```
2. Add the Tailwind PostCSS plugin in `postcss.config.mjs`:
   ```mjs
   const config = {
     plugins: {
       "@tailwindcss/postcss": {},
     },
   };

   export default config;
   ```
3. Create a global stylesheet:
   ```css
   @import "tailwindcss";
   ```
4. Import the stylesheet in a globals file and reference it from `webflow.json`:
   ```ts
   import "./globals.css";
   ```

   ```json
   {
     "library": {
       "globals": "./src/globals.ts"
     }
   }
   ```

## styled-components

1. Install the Webflow utility:
   ```bash
   npm i @webflow/styled-components-utils
   ```
2. Install peer dependencies:
   ```bash
   npm i styled-components react react-dom
   ```
3. Export the decorator from `globals.ts`:
   ```ts
   import { styledComponentsShadowDomDecorator } from "@webflow/styled-components-utils";

   export const decorators = [styledComponentsShadowDomDecorator];
   ```

## Emotion

1. Install the Webflow utility:
   ```bash
   npm i @webflow/emotion-utils
   ```
2. Install peer dependencies:
   ```bash
   npm i @emotion/cache @emotion/react react react-dom
   ```
3. Export the decorator from `globals.ts`:
   ```ts
   import { emotionShadowDomDecorator } from "@webflow/emotion-utils";

   export const decorators = [emotionShadowDomDecorator];
   ```

## Other libraries

Most React component libraries work out of the box, but ensure any global styles are imported via a globals file or Shadow DOM decorators.
