# AGENTS.md - Development Guidelines for landing-wmsas

## Project Overview

This is an Astro + Tailwind CSS landing page project. It uses Astro v6 with Tailwind CSS v4 for styling.

## Commands

### Development
```bash
npm run dev          # Start dev server at localhost:4321
npm run preview      # Preview production build locally
```

### Build
```bash
npm run build        # Build production site to ./dist/
```

### Astro CLI
```bash
npm run astro <cmd>  # Run Astro CLI commands
npm run astro check # Type-check the project
```

### No test or lint scripts are configured

---

## Code Style Guidelines

### File Organization
- `.astro` files go in `src/components/`, `src/layouts/`, or `src/pages/`
- Global styles in `src/styles/global.css`
- Static assets in `public/`

### Astro Components

**Frontmatter (JavaScript/TypeScript):**
- Place imports at the top between `---` fences
- Use `const` for component-level variables
- Define data arrays in frontmatter for template iteration

```astro
---
import Layout from '../layouts/Layout.astro';
import { Icon } from "astro-icon/components";

const items = [
    { label: "Home", icon: "home-icon" },
];
---
```

**Template:**
- Use Tailwind utility classes for all styling
- Use `{expression}` syntax for dynamic content
- Use `{array.map((item) => ())}` for lists
- Use `<slot />` for content projection in layouts

```astro
<div class="col-span-3 space-y-8">
    {items.map((item) => (
        <a href={item.link}>
            <Icon name={item.icon} />
        </a>
    ))}
</div>
```

### Tailwind CSS v4

**Setup:**
- Uses `@import "tailwindcss"` (v4 syntax)
- Define custom colors in `:root` with CSS variables
- Use `@theme inline` to expose custom properties as Tailwind utilities

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

**Usage:**
- Use `text-primary`, `bg-secondary` for custom colors
- Use standard Tailwind classes for spacing, typography, layout
- Use responsive prefixes: `md:`, `lg:`, etc.

### TypeScript

- TypeScript is enabled via `// @ts-check` in config files
- Config extends `astro/tsconfigs/strict`
- No explicit type annotations needed in `.astro` frontmatter for simple cases

### Naming Conventions

- **Files:** kebab-case (e.g., `Home.astro`, `Sidebar.astro`)
- **Components:** PascalCase in imports, kebab-case in file names
- **CSS Variables:** kebab-case with semantic names (e.g., `--primary`)
- **Classes:** Tailwind utilities only (no custom CSS unless necessary)

### Icons

- Use `astro-icon` package
- Import: `import { Icon } from "astro-icon/components";`
- Usage: `<Icon name="icon-name" />`
- Icons stored in `public/images/` or configured icon sets

### Error Handling

- No formal error handling patterns in use
- For production: add try/catch in any client-side script sections
- Use Astro's built-in error pages for 404/500

### Imports

- Always use ESM imports with `.js` extension optional
- Relative imports for local files: `../components/Component.astro`
- Package imports: `astro-icon/components`

---

## Common Patterns

### Layout Component
```astro
---
import '../styles/global.css'
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
        <main class="ml-20 py-8 px-4">
            <slot />
        </main>
    </body>
</html>
```

### Conditional Classes
Use template expressions to conditionally apply classes:
```astro
<div class={`base-class ${isActive ? 'active' : ''}`}>
```

---

## Additional Notes

- Node.js 22.12.0+ required (see `engines` in package.json)
- No ESLint or Prettier configured - code formatting is manual
- No test framework installed
- Project uses Spanish content (labels, text)
