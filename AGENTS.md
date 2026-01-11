 # AGENTS.md for log-lottery
 ## Project Overview
log-lottery is a configurable 3D lottery application built with Vue 3, featuring a 3D sphere visualization using Three.js. It's designed for events like annual company parties, supporting prize configuration, participant management, and customizable UI/media settings. The application uses IndexedDB for local browser storage and supports both web deployment and standalone HTML file distribution.
 ## Build & Development Commands
 ### Essential Commands

- `npm run dev` - Start dev server on localhost:6719
- `npm run build` - Production build with typecheck (vue-tsc --noEmit && vite build)
- `npm run test` - Run all tests with Vitest
- `npm run test:ui` - Run tests with Vitest UI
- `npm run lint` - Lint src directory with ESLint
- `npm run lint:fix` - Auto-fix linting issues
- `npm run preview` - Preview production build
### Running Single Tests
- `vitest run __test__/Button.test.ts` - Run specific test file
- `vitest run -t "test name"` - Run tests matching pattern
- `vitest watch __test__/Button.test.ts` - Watch mode for specific file
## Code Style Guidelines
### TypeScript Configuration
- Strict mode enabled
- Target: ESNext, Module: ESNext
- Path alias: `@/*` maps to `src/*`
- Global Vitest types available via `types: ["vitest/globals"]`
- Use `import type { ... }` for type-only imports
### Vue Component Guidelines
- Use `<script setup lang="ts">` syntax exclusively
- Composition API with auto-imported Vue APIs (ref, computed, watch, etc.)
- Component structure: `src/components/ComponentName/index.vue`
- Use `storeToRefs` from Pinia for reactive store access
- Define props with TypeScript interfaces
- Use DaisyUI + Tailwind CSS utility classes for styling
- Template uses template refs with `ref()` for DOM access
### Pinia Store Conventions
- Use options API with `defineStore('storeName', {...})`
- Structure: state, getters, actions sections
- Store files in `src/store/` (e.g., `globalConfig.ts`, `personConfig.ts`)
- Access stores via `useStore()` composable from `src/store/index.ts`
- Persist configuration with `pinia-plugin-persist` using localStorage
- Reset methods should restore initial state
### Type Definitions
- Interface naming: `I` prefix (e.g., `IPersonConfig`, `IPrizeConfig`, `IMusic`)
- All types in `src/types/storeType.ts`
- Export interfaces with `export interface IName { ... }`
- Import with `import type { IName } from '@/types/storeType'`
### Testing Guidelines
- Framework: Vitest with `@vue/test-utils`
- Test files in `__test__/` directory (root level)
- Use `shallowMount()` for components to avoid rendering child components
- Test structure: `describe('ComponentName', () => { test('scenario', () => {...}) })`
- Use `expect(wrapper.text()).toBe('expected')` for assertions
- Mock external dependencies when needed
- Environment: jsdom with global Vitest types
### Import Organization
- External packages first: `import { ref } from 'vue'`
- Local imports with `@/` alias: `import useStore from '@/store'`
- Auto-imports configured for:
  - Vue Composition API (ref, computed, watch, etc.)
  - Vue Router (useRouter, useRoute)
  - Vue I18n (useI18n)
  - @vueuse/core utilities
- Icon components auto-imported from Element Plus with `Icon` prefix
### Naming Conventions
- Components: PascalCase (e.g., `PlayMusic`, `SvgIcon`)
- Functions: camelCase (e.g., `filterData`, `addOtherInfo`)
- Variables: camelCase
- Constants: UPPER_SNAKE_CASE
- TypeScript interfaces: PascalCase with `I` prefix
- CSS classes: kebab-case for custom classes
- File names: kebab-case for non-component files (e.g., `request.ts`, `index.ts`)
### Error Handling
- Use try-catch blocks for async operations
- Reject promises in error scenarios
- Console.error() for development error logging
- Validate function parameters before processing
- Handle API errors in axios interceptors
### State Management
- Use Pinia stores for global state
- Use ref() for local component state
- Use computed() for derived state
- Use storeToRefs() to maintain reactivity when destructuring stores
- Persist critical user configuration in localStorage
### Styling Guidelines
- Use Tailwind CSS utility classes in templates
- DaisyUI component classes for pre-built UI elements
- Scoped SCSS in `<style lang="scss" scoped>` blocks
- Global SCSS in `src/style/` directory
- Use SCSS variables and mixins via `@use "@/style/global.scss" as *;`
## File Organization
  src/
  ├── api/           # API request configurations
  ├── assets/        # Static assets
  ├── components/    # Reusable Vue components
  ├── hooks/         # Composable functions
  ├── icons/         # SVG icon files
  ├── layout/        # Layout components
  ├── locales/       # i18n translation files
  ├── router/        # Vue Router configuration
  ├── store/         # Pinia stores
  ├── style/         # Global styles
  ├── types/         # TypeScript type definitions
  ├── utils/         # Utility functions
  └── views/         # Page components

### Additional Notes

- Server runs on port 6719
- Proxy configured for `/api` routes to `VITE_BASE_URL`
- Console removed in production builds via terser
- Source maps disabled in production
- Uses Three.js for 3D sphere visualization
- LocalForage for IndexedDB storage (images/music)
