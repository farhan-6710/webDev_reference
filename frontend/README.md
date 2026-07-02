# React TS + shadcn UI starter notes

Use this folder as a copy-ready starter guide for new projects. It captures the repeated setup and conventions so less boilerplate needs to be explained again.

## Default stack

React + TypeScript + shadcn UI

Recommended shared libraries for most projects:

- `react-router`
- `tailwindcss`
- `lucide-react`
- `recharts`
- `sonner`

## First project setup

1. Create the base `src/` structure with `app`, `services`, `features`, and `shared`.
2. Create `router.tsx`.
3. Create a root layout with shared header, navigation, page container, and theme toggle.
4. Set up shadcn UI and put generated primitives in `src/shared/ui/`.
5. Add reusable components you commonly need, such as `ComboBox`, `MultiSelect`, `ConfirmationModal`, loaders, empty states, and page headers.
6. Add `sonner` and wire toast usage for important user actions.
7. Add `src/services/API.ts` and/or `src/services/DB.ts` when the project needs centralized endpoint or database definitions.
8. Create `.env`, `.env.example`, and update `.gitignore`.
9. Update `index.html` title, description, and favicon before shipping.
10. Keep naming conventions and folder structure consistent from the first feature onward.

## `index.html` checklist

Always update:

- `<title>`
- `<meta name="description" />`
- `<link rel="icon" ... />`

Do not leave starter metadata from an old project.

## Folder shape

```txt
src/
  app/
  services/
  features/
    <feature>/
      components/
      constants/
      hooks/
      layouts/
      pages/
      providers/
      types/
      utils/
      config/
  shared/
    components/
    hooks/
    layouts/
    lib/
    types/
    ui/
    utils/
```

Create folders only when needed, but prefer this structure as the default.

## Repeated rules

- Keep API/data access in `services/`.
- Keep pages thin.
- Keep components presentational.
- Keep hooks focused on one concern.
- Keep constants, types, config, and utilities out of `.tsx` files.
- Use precise names everywhere.
- Keep UI/UX consistent across features.
- Prefer ComboBox-style pickers over native dropdown UX when choosing from app data.
- Use confirmation before destructive or irreversible actions.

## Nice defaults to remember

- Global dark mode / light mode toggle
- Shared root layout
- Shared page header pattern
- Shared form field styling
- Shared modal pattern
- Shared table pattern
- Shared toast pattern
- Shared empty/loading/error states

## Before starting any new feature

- Decide the feature name carefully.
- Add route(s) in `router.tsx`.
- Create the feature folder with the standard subfolders.
- Add service functions before wiring the page.
- Reuse shared UI before creating new primitives.
- Keep the first version simple.

## Related docs

- `global/AGENTS.md` — coding rules for agents
- `global/DESIGN.md` — architecture and UI consistency notes
