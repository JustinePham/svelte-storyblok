# Architecture Overview

This document provides a high-level overview of the project structure and how components fit together.

## Project Structure

```
svelte-storyblok/
├── src/                    # Source code directory
│   ├── routes/            # SvelteKit routes and pages
│   ├── components/        # Reusable Svelte components
│   ├── lib/               # Utility functions and helpers
│   └── app.svelte         # Root app component
├── static/                # Static assets (images, fonts, etc.)
├── build/                 # Production build output (generated)
├── node_modules/          # Dependencies (generated)
├── Dockerfile             # Docker configuration for deployment
├── fly.toml              # Fly.io deployment configuration
├── package.json          # Project metadata and dependencies
├── vite.config.ts        # Vite build configuration
├── svelte.config.js      # SvelteKit configuration
├── tailwind.config.js    # Tailwind CSS configuration
├── tsconfig.json         # TypeScript configuration
└── README.md             # Project README
```

## Technology Interactions

### Build Pipeline

```
Source Code (TypeScript/Svelte)
         ↓
    [Vite]  ← Build tool orchestrator
         ↓
    [SvelteKit Compiler] ← Compiles .svelte files to JS
         ↓
    [TypeScript Compiler] ← Type-checks and transpiles TS
         ↓
    [Tailwind CSS] ← Processes utility classes
         ↓
    [PostCSS] ← Final CSS transformations
         ↓
    Production Bundle (JavaScript, CSS, Assets)
         ↓
    build/ Directory
```

### Development Server Flow

1. **Vite Dev Server** starts on port 5173
2. **Watches** for file changes
3. **HMR (Hot Module Replacement)** updates the browser without full reload
4. **Browser DevTools** available for debugging

### Deployment Flow

```
Git Repository
      ↓
   [npm run build] ← Creates production build
      ↓
   [Docker] ← Containerizes app (using Dockerfile)
      ↓
   [Fly.io] ← Deploys container to cloud
      ↓
   Live Application
```

## Key Components

### Svelte Components

Svelte components are `.svelte` files that combine:
- **Markup** — HTML structure
- **Script** — JavaScript logic with reactivity
- **Style** — Scoped CSS (automatically scoped by Svelte)

### Tailwind CSS

Used for utility-first styling:
- Classes are applied directly to HTML elements
- No custom CSS files needed for most styling
- Configuration in `tailwind.config.js` allows customization

### ApexCharts

Integrated for data visualization:
- Import and use chart components in Svelte components
- Bind data to chart instances
- Responsive by default

### SvelteKit Routes

File-based routing system:
- Files in `src/routes/` automatically become routes
- `+page.svelte` — Page component
- `+layout.svelte` — Layout wrapper for multiple pages
- `+server.ts` — API endpoints and server-side logic

## Development Environment

### Local Development

- **Vite Dev Server** provides instant feedback with HMR
- **TypeScript** adds type safety during development
- **Browser DevTools** for inspection and debugging
- **Console** logs for debugging (visible in terminal and browser)

### Production Environment

- **Docker Container** packages the entire application
- **Fly.io** manages scaling, load balancing, and global distribution
- **Node.js Server** serves the compiled application
- **Environment Variables** (defined in fly.toml) configure the app for production

## Data Flow

### From Component to UI

1. **Svelte Component** contains reactive variables
2. **Template** references these variables
3. **User Interaction** triggers event handlers
4. **Variables Update** reactively
5. **Template Re-renders** with new data
6. **Browser Updates** the DOM

### From External Data (API)

1. **Component** calls API endpoint
2. **Server** (if applicable) fetches/processes data
3. **Response** returned to component
4. **Component** stores data in reactive variables
5. **Template** renders the data

## Development Cycle Summary

```
npm run dev
   ↓
Edit Source Files
   ↓
Vite HMR Updates Browser
   ↓
Test Locally
   ↓
npm run build (optional, for testing production)
   ↓
npm run preview (optional, to verify production build)
   ↓
git add . && git commit && git push
   ↓
fly deploy (when ready to deploy)
```

## Configuration Files

| File | Purpose |
|------|---------|
| `vite.config.ts` | Vite build tool settings |
| `svelte.config.js` | SvelteKit framework settings |
| `tailwind.config.js` | Tailwind CSS customization |
| `tsconfig.json` | TypeScript compiler options |
| `fly.toml` | Fly.io deployment configuration |
| `package.json` | Dependencies and npm scripts |
| `Dockerfile` | Container build instructions |

## Performance Considerations

- **Vite** provides fast builds and HMR
- **Svelte** compiles to optimized vanilla JavaScript
- **Tailwind** optimizes CSS by purging unused styles
- **Code Splitting** — SvelteKit automatically splits code for routes
- **Image Optimization** — Store optimized images in `static/`

## Next Steps for Customization

1. **Add Routes** — Create new files in `src/routes/`
2. **Create Components** — Build reusable components in `src/components/`
3. **Add Styling** — Use Tailwind utility classes in templates
4. **Integrate APIs** — Create server-side endpoints in `src/routes/`
5. **Customize Configuration** — Modify `tailwind.config.js`, `vite.config.ts`, etc.

For more detailed information, see the [Development Guide](./DEVELOPMENT.md).
