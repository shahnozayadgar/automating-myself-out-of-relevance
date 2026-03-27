## Project Overview

This project is a frontend-only demo web app that simulates an AI chatbot with agentic plugin capabilities. Users can explore four plugin flows — Excel Q&A, Form Filling, Slide Generation, and Meeting Transcription — through a clickable, guided interface.

Primary users are stakeholders, investors, and product teams who need to experience the product vision before backend development begins.

The product optimizes for:
- realistic interaction feel with mock data
- visible chain-of-thought reasoning and progress states
- clean, minimal UI inspired by ChatGPT/Claude
- modular plugin architecture that maps to real future integrations

Avoid functional backend logic. Prefer scripted flows with polished transitions over working AI.

## Tech Stack & Configuration

- **React 19** with React DOM
- **Vite 8** as build tool and dev server
- **Tailwind CSS 4** via Vite plugin (`@tailwindcss/vite`)
- **Framer Motion** for typing indicators, step reveals, and progress animations
- **Lucide React** for icons
- **ESLint 9** with flat config format
- **@vitejs/plugin-react** using Oxc

Do not introduce:
- TypeScript (plain JSX for speed of iteration)
- Redux or Zustand (useState/useReducer is sufficient)
- styled-components or CSS modules
- Material UI or Ant Design
- React Router (single-page app, no routing needed)
- any backend, API calls, or database

unless explicitly requested.

## Architecture

- `src/components/chat/` — chat container, message bubbles, input bar, typing indicator
- `src/components/plugins/` — one component per plugin (ExcelQA, FormFill, SlideGen, Transcription)
- `src/components/ui/` — reusable primitives (buttons, cards, progress bars, modals)
- `src/data/` — mock data and scripted conversation flows for each plugin
- `src/hooks/` — shared hooks (useTypingAnimation, useStepReveal, useDelayedSequence)
- `src/App.jsx` — main layout, plugin menu, and top-level state

Rules:
- Keep all state client-side with useState/useReducer
- No API calls — all responses come from mock data in `src/data/`
- Each plugin flow is a self-contained module with its own mock script
- Keep animation and timing logic in hooks, not inline in components
- The chat container orchestrates plugin rendering based on the active flow

## Plugin Flow Structure

Each plugin follows the same interaction pattern:
1. User clicks a suggested prompt or plugin card
2. Bot shows a thinking/reasoning sequence (chain-of-thought steps revealed one at a time)
3. Bot indicates which tool it is invoking (e.g. "Searching spreadsheet...")
4. A progress bar or loading state animates
5. Bot renders the final mock result (table, form, slide preview, transcript)

Each flow is defined as a script in `src/data/` containing:
- the trigger prompt text
- an array of reasoning steps (strings revealed sequentially)
- the plugin name and icon
- the mock result component or data to render

## Coding Conventions

- Use JSX strictly — no TypeScript in this project
- Prefer functional components with hooks
- Prefer named exports for shared modules
- Use async/await for any simulated delays (setTimeout wrapped in promises)
- Keep components focused and composable
- Extract repeated logic into custom hooks
- Prefer descriptive variable names over abbreviations
- Add comments only when intent is non-obvious
- Do not leave dead code or commented-out blocks

## UI and Design System Rules

- Use shadcn/ui-inspired primitives as the default foundation
- Prefer spacious layouts and strong visual hierarchy
- Use restrained color — rely on typography, spacing, and contrast
- Prefer 8px spacing rhythm
- Buttons should have clear primary/secondary hierarchy
- Every interactive element must have visible hover, focus, and disabled states
- Chat bubbles should clearly distinguish user vs bot messages
- Plugin result cards should feel embedded in the conversation, not overlaid

## File Placement Rules

- Add chat UI components (message bubbles, input bar, typing indicator) to `src/components/chat/`
- Add plugin flow components (ExcelQA, FormFill, SlideGen, Transcription) to `src/components/plugins/`
- Add reusable primitives (buttons, cards, progress bars, modals) to `src/components/ui/`
- Add scripted conversation flows and mock response data to `src/data/`
- Add shared hooks (useTypingAnimation, useStepReveal, useDelayedSequence) to `src/hooks/`
- Keep the main app layout and plugin menu in `src/App.jsx`
- Do not create a new component for one-off usage — inline it or extend an existing one
- Do not scatter mock data across component files — centralize it in `src/data/`
- Prefer editing an existing plugin component over creating a near-duplicate
- Keep animation and timing logic in hooks, not directly in component bodies

## Quality Checks

Before considering a task complete:
- Run lint
- Visually verify in the browser across the full flow
- Check that all four plugin flows render without errors
- Verify loading, thinking, and result states appear correctly
- Ensure transitions and animations feel smooth, not janky

Do not add unit tests or heavy test scaffolding — this is a demo prototype. Visual QA is the primary quality gate.

## Commands

```bash
npm run dev        # start Vite dev server
npm run build      # production build to dist/
npm run preview    # preview production build locally
npm run lint       # run ESLint
```