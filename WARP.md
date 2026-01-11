# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

log-lottery is a configurable 3D lottery application built with Vue 3, featuring a 3D sphere visualization using Three.js. It's designed for events like annual company parties, supporting prize configuration, participant management, and customizable UI/media settings. The application uses IndexedDB for local browser storage and supports both web deployment and standalone HTML file distribution.

## Common Commands

### Development
```bash
# Start dev server (default: localhost:6719)
pnpm dev
# or
npm run dev
```

### Building
```bash
# Standard production build (outputs to dist/)
pnpm build
# or
npm run build

# Pre-build mode (preserves chunk names)
pnpm build:pre
# or
npm run build:pre

# Standalone file build (outputs to dist-file/, can open index.html directly)
pnpm build:file
# or
npm run build:file
```

### Testing
```bash
# Run tests with Vitest
pnpm test
# or
npm run test

# Run tests with UI
pnpm test:ui
# or
npm run test:ui
```

### Linting
```bash
# Check for linting issues
pnpm lint
# or
npm run lint

# Auto-fix linting issues
pnpm lint:fix
# or
npm run lint:fix
```

### Docker
```bash
# Build Docker image
docker build -t log-lottery .

# Run container (access at http://localhost:9279/log-lottery/)
docker run -d -p 9279:80 log-lottery
```

## Architecture

### State Management (Pinia Stores)

The application uses Pinia with persistence plugin for state management. All stores are located in `src/store/`:

- **data.ts**: Contains default data (person list, prize list, music list) with hardcoded initial values
- **globalConfig.ts**: Manages global UI settings (theme, colors, card dimensions, music/image lists, language)
- **personConfig.ts**: Manages participant data and winner tracking
- **prizeConfig.ts**: Manages prize configurations and lottery state
- **system.ts**: System-level state management

### Routing Structure

The application uses Vue Router with history mode (hash mode for `build:file`). Main route structure:

- `/log-lottery/home` - Main lottery interface
- `/log-lottery/demo` - Demo page
- `/log-lottery/config` - Configuration panel with nested routes:
  - `/person` - Participant configuration (all participants, winners list)
  - `/prize` - Prize configuration
  - `/global` - Global settings (UI config, image/music management)
  - `/readme` - Instructions

### Key Technologies

- **Vue 3** with Composition API
- **Three.js** - 3D sphere rendering and animations
- **Tween.js** - Animation tweening
- **IndexedDB** (via localforage) - Client-side data persistence
- **Pinia** - State management with persistence
- **Vue I18n** - Internationalization support
- **DaisyUI + Tailwind CSS** - UI framework
- **XLSX** - Excel import/export for participant lists
- **Vite** - Build tool with optimized chunking strategy

### Build Modes

The project supports three build modes controlled by Vite environment:

1. **default**: Standard web build with base path `/log-lottery/`
2. **prebuild**: Named chunks for better debugging
3. **file**: Hash-based routing and relative paths for standalone HTML file usage, includes legacy browser polyfills

### Data Persistence

All application data is persisted locally in the browser using:
- Pinia stores with `pinia-plugin-persist`
- IndexedDB (via localforage) for images and audio files

### Component Structure

- **Layout** (`src/layout/`): Main application shell
- **Views** (`src/views/`):
  - `Home/` - Main lottery display and interaction
  - `Config/` - Configuration panels (Person, Prize, Global settings, Readme)
  - `Demo/` - Demo/testing views
- **Components** (`src/components/`):
  - `DaiysuiTable` - Table component wrapper
  - `ImageSync` - Image synchronization component
  - `PlayMusic` - Music player integration
  - `StarsBackground` - Animated background
  - `SvgIcon` - SVG icon wrapper
  - `ToTop` - Scroll to top button
  - `NumberSeparate` - Number display component

### Utilities (`src/utils/`)

- `auth.ts` - Authentication utilities
- `color.ts` - Color manipulation functions
- `file.ts` - File handling utilities
- `index.ts` - Common utility functions
- `store.ts` - Store helper utilities

## Development Guidelines

### Path Aliases

Use the `@/` alias for imports, which resolves to `src/`:
```typescript
import Component from '@/components/Component.vue'
import { useStore } from '@/store'
```

### Build Configuration

The build process includes:
- Terser minification with console/debugger removal in production
- Gzip compression for files > 10KB
- Code splitting by node_modules
- Bundle visualization (generates `test.html`)

### Styling

- Global SCSS variables available via `@/style/global.scss`
- Tailwind CSS with DaisyUI components
- PostCSS with autoprefixer

### Type Checking

TypeScript compilation must pass before building:
```bash
vue-tsc --noEmit
```

### Icon System

- Icons auto-imported from `src/icons/` directory
- Iconify icons available with `Icon` prefix (ep collection enabled)
- SVG icons registered via `virtual:svg-icons-register`

### Testing Setup

- Vitest with jsdom environment
- Testing Library for Vue
- Global test utilities available
- Tests located in `__test__/` directory

### Internationalization

- Default language: Browser language detection
- Supported languages configured in `src/locales/`
- Language stored in globalConfig store

## Important Notes

- Dev server runs on port 6719 with host binding to 0.0.0.0
- All lottery data (participants, prizes, winners) persists locally in the browser
- Excel templates used for bulk participant import/export
- Background images and music files stored in IndexedDB
- The project uses ESLint with @antfu/eslint-config
- Three.js is mounted globally as `$THREE` on app instance
