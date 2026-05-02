# Discovery Guide: Web Frontend

## Step 1: Framework

**Slot: framework**
- Check `package.json` dependencies:
  - `react` → React
  - `vue` → Vue
  - `@angular/core` → Angular
  - `svelte` → Svelte
- Check config files:
  - `vite.config.ts` → Vite
  - `next.config.js` → Next.js
  - `nuxt.config.ts` → Nuxt

## Step 2: State Management

**Slot: state-management**
- Search `package.json`:
  - `redux`, `@reduxjs/toolkit` → Redux
  - `zustand` → Zustand
  - `pinia` → Pinia
  - `vuex` → Vuex
  - `@ngxs/store` → NGXS
- Search code for imports

## Step 3: Styling

**Slot: styling**
- Check dependencies:
  - `tailwindcss` → Tailwind
  - `styled-components` → Styled Components
  - `sass`, `node-sass` → SCSS
  - `less` → Less
- Check file extensions: `.css`, `.scss`, `.less`, `.module.css`

## Step 4: Testing

**Slot: test-framework**
- Check dependencies:
  - `jest` → Jest
  - `vitest` → Vitest
  - `cypress` → Cypress
  - `@playwright/test` → Playwright
  - `@testing-library/*` → Testing Library

## Step 5: Architecture

**Slot: architecture-pattern**
- Check directory structure:
  - `components/`, `pages/`, `hooks/` → Component-based
  - `tasks/`, `shared/` → Feature-based
  - `src/app/`, `src/environments/` → Angular

---

## Discovery Summary

```
Discovery Complete: Web Frontend Profile

Project: {project-name}
  Framework: {framework}

Stack:
  State: {state-management}
  Styling: {styling}
  Routing: {routing}
  Testing: {test-framework}

Architecture:
  Pattern: {architecture-pattern}
```
