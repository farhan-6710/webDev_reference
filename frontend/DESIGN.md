# Design — React TS + shadcn UI

This file explains the default app shape for future projects. Keep architecture simple, consistent, and easy to grow.

## Core idea

Data should flow in one direction:

```txt
page -> hook -> service
  -> component (presentational)
```

- **Pages** compose layouts, hooks, and components.
- **Hooks** manage one concern: state, effects, filters, form logic, or actions.
- **Services** own API/data access and shared clients.
- **Components** focus on UI only.
- **Utils** stay pure and reusable.

## App structure

- `app/` holds bootstrap code, app providers, root layout, route setup, and app-level config.
- `services/` holds API clients, endpoint maps, database maps, and all data-access functions.
- `features/` holds domain-specific code using the same internal structure per feature.
- `shared/` holds reusable UI, hooks, layouts, types, and utilities used across features.

## Consistent UI

- Use one shared visual language for spacing, typography, radius, shadows, and interaction states.
- Use shadcn UI as the base system.
- Use `lucide-react` for icons and `recharts` for charts.
- Use a root layout with shared header, navigation, and page container rules.
- Add a global dark mode / light mode toggle in the header.
- Prefer ComboBox-based pickers over native dropdown UX to keep the UI polished and consistent.
- Reuse the same page header, form, modal, table, empty state, loading state, and toast patterns throughout the app.

## Routing and layouts

- Create `router.tsx` early.
- Keep page routes feature-based and easy to scan.
- Put repeated shell UI in a root layout instead of duplicating wrappers on every page.
- Add feature-level layouts only when a feature has truly different navigation or page chrome.

## Services and config

- Keep service files plain and predictable.
- Use `API.ts` for base URLs, endpoint definitions, request methods, or HTTP client setup when needed.
- Use `DB.ts` for table names, collection names, select strings, or schema constants when needed.
- Avoid scattering endpoint strings, route fragments, and table names across the app.

## Reusable setup

Start each project with these shared basics:

- `index.html` title, description, and favicon
- `.env` and `.env.example`
- `.gitignore`
- router setup
- root layout
- global header
- theme toggle
- shadcn UI setup
- `sonner` toast setup
- reusable components like `MultiSelect`, `ComboBox`, dialogs, loaders, and empty states

## Quality bar

- Prefer small files and obvious patterns.
- Keep naming precise and stable.
- Add only the abstractions the project already needs.
- If the same setup repeats across projects, document it here or in `README.md` instead of re-explaining it every time.
