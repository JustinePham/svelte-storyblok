# Development Guide

This document outlines the development workflow, technology stack, and deployment process for this Svelte Storyblok project.

## Table of Contents

1. [Technology Stack](#technology-stack)
2. [Local Development Workflow](#local-development-workflow)
3. [Build and Preview](#build-and-preview)
4. [Version Control](#version-control)
5. [Deployment](#deployment)

---

## Technology Stack

### Core Framework

- **Svelte** — A modern JavaScript framework for building user interfaces with a reactive, component-based architecture. Compiles to vanilla JavaScript for optimal performance.
- **SvelteKit** — The official application framework for Svelte, providing routing, server-side rendering (SSR), and optimized builds.
- **Node.js** — JavaScript runtime environment used for running development servers, build tools, and the deployed application backend.

### Styling & UI

- **Tailwind CSS** — A utility-first CSS framework used for rapid, responsive UI development. Configured in `tailwind.config.js` and includes pre-built utility classes for styling components.
- **PostCSS** — CSS processing tool used in conjunction with Tailwind CSS for transformations and optimizations. Configuration in `_postcss.config.js`.

### Data Visualization

- **ApexCharts** — A modern charting library used for creating interactive, responsive charts and graphs in the application. Provides various chart types (line, bar, pie, area, etc.).

### Build & Development Tools

- **Vite** — A high-performance build tool and development server. Provides fast hot module replacement (HMR) for instant feedback during development. Configuration in `vite.config.ts`.
- **TypeScript** — Superset of JavaScript that adds static typing for better developer experience and error catching. Configuration in `tsconfig.json`.

### Deployment Platform

- **Fly.io** — Platform-as-a-Service (PaaS) for deploying and running the application. Handles infrastructure, scaling, and global distribution. Configuration in `fly.toml`.

### Version Control

- **Git** — Distributed version control system for tracking code changes, collaboration, and managing the repository history.

### Docker

- **Docker** — Containerization technology used to package the application for deployment. Configuration in `Dockerfile` and `.dockerignore`.

---

## APIs Used

### Storyblok CMS API

- **Purpose** — Fetches dynamic content and data from the Storyblok Content Management System
- **Endpoint** — Storyblok GraphQL and REST API
- **Authentication** — API token (stored in environment variables)
- **Data Retrieved** — Page content, components, assets, and metadata
- **Documentation** — [Storyblok API Docs](https://www.storyblok.com/docs/api)

### NASA API

- **Purpose** — Retrieves precipitation data for weather and climate visualization
- **Endpoint** — NASA's data API
- **Authentication** — API key (stored in environment variables)
- **Data Retrieved** — Precipitation data used for charts and analysis
- **Documentation** — [NASA API Documentation](https://api.nasa.gov)

**Environment Variables Required:**
- `STORYBLOK_API_KEY` — Storyblok API token
- `NASA_API_KEY` — NASA API key

---

---

## Local Development Workflow

### Prerequisites

Ensure you have the following installed:
- Node.js (v16 or higher recommended)
- npm (comes with Node.js)
- Git

### Starting Development

1. **Clone the repository** (if not already cloned):
   ```bash
   git clone <repository-url>
   cd svelte-storyblok
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Start the development server**:
   ```bash
   npm run dev
   ```

   This command:
   - Starts a local development server (typically at `http://localhost:5173`)
   - Enables hot module replacement (HMR) for instant feedback when you save files
   - Watches for file changes and automatically rebuilds as needed

4. **Optionally, open the app in your browser automatically**:
   ```bash
   npm run dev -- --open
   ```

### Development Best Practices

- **Keep the dev server running** — The development server automatically reloads when you make changes to your code.
- **Use browser DevTools** — Open your browser's developer tools to inspect elements, debug JavaScript, and monitor network requests.
- **Check TypeScript errors** — TypeScript will catch type mismatches. Pay attention to any type errors in your editor or build output.

---

## Build and Preview

### Building for Production

When you've finished implementing a feature or are ready to test the production build:

```bash
npm run build
```

This command:
- Compiles Svelte components and TypeScript to optimized JavaScript
- Processes CSS with Tailwind and PostCSS
- Bundles assets with Vite
- Creates a production-ready build in the `build/` directory

### Previewing the Production Build

To see what your application will look like when deployed (before deploying to Fly.io):

```bash
npm run preview
```

This command:
- Starts a local server serving the production build
- Allows you to test the app as it will appear in production
- Useful for catching build-time issues before deployment

---

## Version Control

### Using Git

1. **Check status**:
   ```bash
   git status
   ```

2. **Stage changes**:
   ```bash
   git add <file>
   # or stage all changes
   git add .
   ```

3. **Commit your changes**:
   ```bash
   git commit -m "Descriptive message of what changed"
   ```

4. **Push to the repository**:
   ```bash
   git push origin <branch-name>
   ```

### Workflow Recommendations

- **Commit frequently** — Make small, atomic commits with clear messages.
- **Use descriptive commit messages** — Describe what changed and why.
- **Create feature branches** — Work on features in separate branches before merging to main.
- **Push regularly** — Push your changes to save them in the repository.

---

## Deployment

### Prerequisites

- Fly.io account and the `fly` CLI tool installed
- Docker (Fly.io uses Docker to containerize your application)
- Proper authentication with Fly.io (`fly auth login`)

### Deploying to Fly.io

1. **Ensure your changes are committed**:
   ```bash
   git status
   # All changes should be committed before deploying
   ```

2. **Build the application** (optional, but recommended):
   ```bash
   npm run build
   ```

3. **Deploy to Fly.io**:
   ```bash
   fly deploy
   ```

   This command:
   - Uses the Dockerfile to build a container image
   - Pushes the image to Fly.io's registry
   - Deploys the application to your Fly.io app
   - Makes the application available at your configured domain

### Post-Deployment

- **Check deployment status**:
  ```bash
  fly status
  ```

- **View logs**:
  ```bash
  fly logs
  ```

- **Verify the deployment** — Visit your application URL to ensure it's working correctly.

### Configuration

Deployment settings are configured in `fly.toml`:
- App name
- Build configuration
- Port settings
- Environment variables
- Region and scaling options

For more details on configuring Fly.io, refer to [Fly.io Documentation](https://fly.io/docs/).

---

## Common Workflows

### Adding a New Feature

1. Create a new branch: `git checkout -b feature/feature-name`
2. Start the dev server: `npm run dev`
3. Implement your feature
4. Test locally
5. Commit and push: `git add . && git commit -m "..." && git push origin feature/feature-name`
6. Create a pull request (if using GitHub)

### Before Deploying

1. Ensure all changes are committed: `git status`
2. Build the app: `npm run build`
3. Preview the build: `npm run preview`
4. Verify everything looks correct
5. Deploy: `fly deploy`

### Debugging Issues

- **Dev server issues** — Check terminal output for errors, restart with `npm run dev`
- **Build errors** — Review error messages from `npm run build`
- **Type errors** — Check `tsconfig.json` and fix TypeScript errors
- **Deployment issues** — Check `fly logs` for error messages

---

## Resources

- [Svelte Documentation](https://svelte.dev)
- [SvelteKit Documentation](https://kit.svelte.dev)
- [Tailwind CSS Documentation](https://tailwindcss.com)
- [Vite Documentation](https://vitejs.dev)
- [ApexCharts Documentation](https://apexcharts.com)
- [Fly.io Documentation](https://fly.io/docs/)
- [TypeScript Documentation](https://www.typescriptlang.org)
