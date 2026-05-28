# Project Setup with Shadcn UI

This guide outlines the steps to set up your project with Shadcn UI, including Tailwind CSS integration and path aliases.

1. Install Dependencies

```bash
bun add tailwindcss @tailwindcss/vite
bun add -D @types/node

2. Update src/index.css

code
CSS
@import "tailwindcss";

3. Update tsconfig.json

code
JSON
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

4. Update tsconfig.app.json

code
JSON
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}

5. Update vite.config.ts

code
TypeScript
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

6. Initialize Shadcn UI

After completing the above steps, run the Shadcn UI initialization command:
code
Bash
bunx --bun shadcn@latest init

7. Add Components

Once Shadcn UI is set up, you can start adding components, for example:
code
Bash
bunx --bun shadcn@latest add button
