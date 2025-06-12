---
applyTo: '**'
---

# Copilot Project Instructions Template

This document describes the coding standards, patterns, and best practices. Use this as a template for future TypeScript/Node.js projects to ensure consistency, maintainability, and high code quality.

---

## 1. Project Structure
- **Source code**: Place all TypeScript source files in a `out/` directory.
- **Build output**: Output compiled files to an `out/` directory.
- **Scripts**: Place CLI entry points in a `bin/` directory.
- **Configuration**: Store configuration files (e.g., `tsconfig.json`, `.vscode/`) at the project root.
- **Documentation**: Include `README.md` and `LICENSE` at the root.
- **Ignore files**: Include `.gitignore` and `.npmignore` files at the root.

## 2. TypeScript Configuration
- Use a strict `tsconfig.json`:
  - `strict: true`
  - `esModuleInterop: true`
  - `forceConsistentCasingInFileNames: true`
  - `skipLibCheck: true`
  - `target: es2022`, `module: nodenext`, `lib: ["es2022"]`
  - Set `rootDir` to `out` and `outDir` to `out`.
- Exclude `node_modules` from compilation.
- IMPORTANT: Use `out` for `rootDir` and `outDir`. DO NOT ADD ANYTHING INTO `src` folder!

## 3. Code Style & Conventions
- **Header**: Add a copyright/license header to every source file.
- **Imports**: Use ES6 import syntax. Group external, then internal imports.
- **Constants**: Use `const` for values that do not change, `let` for mutable variables. Avoid `var`.
- **Enums & Types**: Use TypeScript `enum`, `interface`, and `type` for structure and clarity.
- **Classes**: Use classes for encapsulation. Prefer singletons for shared state.
- **Functions**: Use `async/await` for asynchronous code. Always handle errors with `try/catch`.
- **Error Handling**: Log errors with context. Provide troubleshooting steps in error messages.
- **Comments**: Use JSDoc-style comments for public APIs. Use inline comments sparingly and only when necessary.
- **Formatting**: Use a consistent code formatter (e.g., Prettier). Indent with 4 spaces or project standard.

## 4. Libraries & Dependencies
- Use these dependencies exactly as specificed in the `package.json`:
```
"dependencies": {
  "@vscode/vscode-perf": "^0.0.19",
  "chalk": "^5.x",
  "commander": "^14.x",
  "fflate": "^0.8.2",
  "follow-redirects": "^1.15.9",
  "open": "^10.1.2",
  "progress": "^2.0.3",
  "prompts": "^2.4.2",
  "tree-kill": "^1.2.2",
  "vscode-uri": "^3.1.0"
},
"devDependencies": {
  "@types/follow-redirects": "^1.x",
  "@types/node": "22.x",
  "@types/progress": "^2.0.7",
  "@types/prompts": "^2.x",
  "typescript": "5.x"
}
```
- Use `@types/*` packages for type safety in devDependencies.
- Use node.js 22.x.
- Pin dependency versions for reproducibility.

## 5. Patterns & Best Practices
- **Singletons**: Export a single instance for shared services (e.g., `export const storage = new Storage();`).
- **Async Initialization**: Use private `whenReady`/`init()` patterns for async setup.
- **Separation of Concerns**: Split code into logical modules (e.g., `builds.ts`, `index.ts`, `launcher.ts`).
- **CLI Entrypoint**: Use a single entry file (e.g., `index.ts`) and export a function for CLI execution.
- **Error Reporting**: Provide actionable error messages and troubleshooting steps.
- **Platform Awareness**: Detect and handle platform differences (e.g., file paths, process management).
- **Configuration**: Store runtime configuration in a central object (e.g., `CONFIG`).
- **Testing**: (If applicable) Use npm scripts for test automation.

## 6. NPM Scripts
- `build`/`compile`: Compile TypeScript sources.
- `watch`: Watch and recompile on changes.
- `prepare`: Build before publishing.
- Use `bin` field in `package.json` for CLI tools.

## 7. VS Code Workspace Settings
- Exclude build output, node_modules, and test folders from search and file explorer.
- Use tasks for background TypeScript compilation.
- Configure launch settings for debugging CLI entrypoints.

## 8. Documentation
- Provide a clear `README.md` with:
  - Project purpose
  - Requirements (e.g., Node.js version)
  - Usage examples
  - Help/CLI options
- Include support and security policies.

---

## Example File/Folder Layout
```
project-root/
  out/
    index.ts
    ...
  bin/
    vscode-sanity
  .gitignore
  .npmignore
  package.json
  tsconfig.json
  README.md
  LICENSE
  .vscode/
```

*Generated from the vscode-bisect project, June 2025.*
