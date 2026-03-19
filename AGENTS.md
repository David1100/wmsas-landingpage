# AGENTS.md - Development Guidelines for landing-wmsas

## Project Overview

Astro v6 + Tailwind CSS v4 landing page for a solar energy company. Content is in Spanish.

### Tech Stack
- **Framework:** Astro v6 (static site generation)
- **Styling:** Tailwind CSS v4 with custom theme colors
- **Icons:** `astro-icon` package (SVG icons)
- **Build:** Vite (via `@tailwindcss/vite` plugin)
- **Runtime:** Node.js 22.12.0+

---

## Commands

```bash
npm run dev        # Dev server at localhost:4321
npm run build      # Production build to ./dist/
npm run preview    # Preview production build locally
npm run astro <cmd>    # Run Astro CLI directly
npm run astro check    # Type-check the project
```

No test or lint framework is configured.

---

## File Organization

```
src/
  components/    # .astro reusable UI components
  layouts/       # Page layout wrappers (contain <slot />)
  pages/         # Route pages (index.astro, etc.)
  styles/
    global.css   # Tailwind imports + custom CSS variables + global styles
public/
  images/        # Static images (backgrounds, icons, etc.)
  favicon.*      # Site favicon
```

---

## Astro Components

### Frontmatter (between `---` fences)

- Imports at the top
- Define data arrays or component-level `const` variables
- TypeScript is available; config extends `astro/tsconfigs/strict`

```astro
---
import Layout from '../layouts/Layout.astro';
import { Icon } from "astro-icon/components";

const features = [
    { label: "Ahorro", icon: "icon-ahorro" },
    { label: "Energía", icon: "icon-energia" },
];
---

<div class="grid grid-cols-3 gap-4">
    {features.map((item) => (
        <div class="flex items-center gap-2">
            <Icon name={item.icon} class="w-6 h-6" />
            <span>{item.label}</span>
        </div>
    ))}
</div>
```

### Template Syntax
- `{expression}` for dynamic content
- `{array.map((item) => (<JSX-like />))}` for lists — note the parentheses, not braces
- `<slot />` for content projection in layouts
- `{/* JSX comment style */}` for comments in template

### Layout Pattern
```astro
---
import '../styles/global.css';
import Sidebar from "../components/Sidebar.astro";
---

<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width" />
        <title>Page Title</title>
    </head>
    <body>
        <Sidebar />
        <main class="pl-15 fondo">
            <slot />
        </main>
    </body>
</html>
```

---

## Tailwind CSS v4

### Setup (global.css)
Uses `@import "tailwindcss"` (v4 syntax). Custom colors are defined with CSS variables and exposed via `@theme inline`.

```css
@import "tailwindcss";

:root {
    --primary: #393C7A;
    --secondary: #d0d817;
}

@theme inline {
    --color-primary: var(--primary);
    --color-secondary: var(--secondary);
}
```

### Usage
- `text-primary`, `bg-secondary`, `border-primary`, etc.
- Responsive prefixes: `md:`, `lg:`, `xl:`
- Arbitrary values: `[opacity:0.1]`, `h-[62.5]` (Astro supports this)

### Global Utility Classes
Some global classes exist in `global.css` beyond Tailwind:
- `.fondo` — full-viewport background image with fixed attachment and 0.1 opacity overlay
- `.MuiChartsLegend-label` — overrides for chart legends
- Global `button { cursor: pointer; }` reset

---

## TypeScript

- Enabled via `// @ts-check` in config files
- No explicit type annotations needed in `.astro` frontmatter for simple cases
- If adding logic, prefer `const` and inferred types

---

## Naming Conventions

| Thing | Convention | Example |
|---|---|---|
| .astro files | kebab-case | `Home.astro`, `SavingsCalculator.astro` |
| Component imports | PascalCase | `import SavingsCalculator` |
| CSS variables | kebab-case | `--primary`, `--secondary` |
| Tailwind classes | only utility classes | `text-primary`, `bg-secondary` |
| Custom CSS classes | kebab-case | `.fondo`, `.chart-legend` |

---

## Icons

Use the `astro-icon` package:
```astro
import { Icon } from "astro-icon/components";
<Icon name="icon-name" class="w-6 h-6" />
```
Icons are loaded from `public/images/` or a configured icon set in `astro.config.mjs`.

The icon plugin is configured in both `vite.plugins` and `integrations`:
```js
// astro.config.mjs
import icon from "astro-icon";
export default defineConfig({
  vite: { plugins: [tailwindcss(), icon()] },
  integrations: [icon()],
});
```

---

## Conditional Classes

Use template expressions:
```astro
<div class={`base-class ${isActive ? 'active' : ''}`}>
```

---

## Error Handling

- No formal patterns in use
- For client-side script sections, use try/catch
- Use Astro's built-in error pages (404.astro, 500.astro) for routing errors

---

## Astro Config Notes

- TypeScript config extends `astro/tsconfigs/strict`
- Vite plugins: `tailwindcss()` (from `@tailwindcss/vite`) and `icon()`
- No SSR configured — this is a static site
