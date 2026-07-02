# Agent guidelines — React TS + shadcn UI

Use this file as the rulebook for future React + TypeScript + shadcn UI projects. Keep changes small, readable, and consistent.

## Code style

- **Least code, more output** — prefer the smallest change that solves the problem.
- **Beginner-friendly** — one function, one job. Readable code over clever abstractions.
- **Consistent naming** — be precise and predictable with file, folder, variable, hook, and component names.
- **Maintainable & modular** — use feature-based folders and shared building blocks only when reused.
- **Keep setup reusable** — avoid project-specific hacks in shared structure.

## Where code goes

```txt
src/
  app/           app bootstrap, providers, root layout, router
  services/      API/data layer and shared clients
  features/<feature>/
    components/  presentational UI
    constants/   static values, labels, config, enums
    hooks/       one concern per hook
    layouts/     feature-level layouts when needed
    pages/       route pages that compose hooks + components
    providers/   context providers + context files
    types/       types.ts for domain, components.ts for props
    utils/       pure helpers, formatters, validators
    config/      feature-specific config
  shared/
    components/  reusable app-level components
    hooks/       reusable hooks
    layouts/     shared layouts
    lib/         helpers like className utils, api clients
    types/       shared types
    ui/          shadcn components
    utils/       shared pure utilities
```

- Keep domain code inside the feature that owns it.
- Use `shared/` only for code reused across features.
- Do not bury types, constants, or config inside `.tsx` files.

## Services (data layer)

- All API/data access lives in `src/services/`.
- Create a small shared config file such as `src/services/API.ts` for endpoints, methods, base URLs, or shared HTTP clients.
- If the project uses a database SDK, create a file like `src/services/DB.ts` to define table names, select strings, collections, or query keys. Do not inline them across the app.
- One service function = one job: build request/query -> run it -> throw/handle error -> map result to a typed shape.
- Keep services framework-agnostic. UI components and pages should never hold raw request logic.

## Components & hooks

- Keep components presentational; move state, effects, and actions into hooks.
- One concern per hook. Prefer small hooks that compose cleanly.
- Pages should mostly compose layouts, hooks, and components.
- Create reusable primitives for repeated UI, such as `MultiSelect`, `ComboBox`, dialogs, tables, empty states, and loaders.
- Prefer named exports.
- Split files when they become hard to scan.

## UI & UX

- Use shadcn UI components from `src/shared/ui/`.
- Use `sonner` for toasts on create, update, delete, save, submit, connect, disconnect, and other important user actions.
- Use `ConfirmationModal` before important or irreversible actions: sign out, delete/remove, disconnect, reset, discard unsaved work, or similar.
- Keep spacing, typography, button sizes, and card patterns consistent across the app.
- Use a global header with a **dark mode / light mode toggle**.
- Use a root layout so shared chrome is defined once, not repeated on every page.
- Use `lucide-react` for icons.
- Use `recharts` for charts.
- Prefer **ComboBox** over native dropdown/select patterns where a richer picker is needed. Keep the UI consistent with shadcn styling.
- Keep forms, modals, tables, filters, and page headers visually consistent across features.

## Naming

- Components: PascalCase and file name matches export.
- Hooks: `useXxx`.
- Pages: `FeatureNamePage`, `FeatureNameManagementPage`, or another precise route-based name.
- Providers: `XxxProvider`; context file: `xxxContext.ts` or `XxxContext.ts`.
- Constants: `SCREAMING_SNAKE_CASE` for primitives, camelCase for grouped config objects.
- Types: PascalCase. Props end with `Props`.
- Utilities: verb-based or purpose-based names like `formatCurrency`, `buildRoute`, `validateForm`.

## Don't

- Do not put API calls directly in pages or components.
- Do not mix unrelated concerns in one hook or one file.
- Do not add wrapper components or abstractions without clear reuse.
- Do not use inconsistent naming, vague folder names, or duplicate utilities.
- Do not leave title, favicon, metadata, theme, router, or toast setup half-finished.
- Do not use native-looking dropdown UX when the app already uses styled shadcn pickers.

## Before finishing

1. App title, description, and favicon are updated in `index.html`.
2. Router is created in `router.tsx` and wired through the app root.
3. Root layout and global header exist, including theme toggle.
4. Feature folders include `components`, `constants`, `hooks`, `pages`, `providers`, `types`, `utils`, `layouts`, and `config` when needed.
5. Shared reusable components are extracted only when truly reused.
6. `src/services/API.ts` and/or `src/services/DB.ts` are created when the project needs them.
7. `sonner` toasts and `ConfirmationModal` are used for important actions.
8. `.env`, `.env.example`, and `.gitignore` are correctly set up.
9. Imports, routes, and naming stay consistent across the project.
10. Lint and build pass.
