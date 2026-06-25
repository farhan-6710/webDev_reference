# Shadcn UI Setup with Vite + React + TypeScript

This guide walks through setting up **Shadcn UI** with:

* Vite
* React
* TypeScript
* Tailwind CSS v4
* Path aliases (`@/*`)

---

# 1. Install Dependencies

```bash
bun add tailwindcss @tailwindcss/vite
bun add -D @types/node
```

---

# 2. Configure Tailwind CSS

Update `src/index.css`:

```css
@import "tailwindcss";
```

---

# 3. Configure Path Aliases

Update `tsconfig.json`:

```json
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

---

# 4. Update `tsconfig.app.json`

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

---

# 5. Configure Vite

Update `vite.config.ts`:

```ts
import path from "path"
import tailwindcss from "@tailwindcss/vite"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

---

# 6. Initialize Shadcn UI

Run the initialization command:

```bash
bunx --bun shadcn@latest init
or
bunx --bun shadcn@latest init --preset b4VkddTAPa
```

---

# 7. Add Components

Example:

```bash
bunx --bun shadcn@latest add button
```

---

# Notes

* This setup uses **Tailwind CSS v4**.
* The `@/*` alias allows cleaner imports:

```ts
import Button from "@/components/Button"
```

Instead of:

```ts
import Button from "../../components/Button"
```
