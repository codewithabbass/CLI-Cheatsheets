<div align="center">

# 📦 npm, Yarn, pnpm & Bun Commands

</div>

[← Back to Home](../README.md)

---

## Table of Contents

- [Initialization](#-initialization)
- [Installing Packages](#-installing-packages)
- [Removing Packages](#-removing-packages)
- [Updating Packages](#-updating-packages)
- [Global Packages](#-global-packages)
- [Scripts](#-scripts)
- [Version Management](#-version-management)
- [Publishing](#-publishing)
- [Workspaces (Monorepo)](#-workspaces-monorepo)
- [Audit & Security](#-audit--security)
- [Cache & Config](#-cache--config)
- [pnpm](#-pnpm)
- [Bun](#-bun)

---

## 🚀 Initialization

| npm | Yarn |
|-----|------|
| `npm init` | `yarn init` |
| `npm init -y` | `yarn init -y` |
| `npm init --scope=@myorg` | |

```bash
npm init                  # Interactive project setup
npm init -y               # Accept all defaults
```

---

## 📥 Installing Packages

### Install all dependencies

```bash
npm install               # Install from package.json
npm i                     # (short alias)

yarn install              # Same with Yarn
yarn                      # (short alias)
```

---

### Install specific package

| Action | npm | Yarn |
|--------|-----|------|
| Add dependency | `npm install lodash` | `yarn add lodash` |
| Add dev dependency | `npm install -D eslint` | `yarn add -D eslint` |
| Add peer dependency | `npm install --save-peer react` | `yarn add --peer react` |
| Install exact version | `npm install lodash@4.17.21` | `yarn add lodash@4.17.21` |
| Install version range | `npm install lodash@^4.0.0` | `yarn add lodash@^4.0.0` |

```bash
npm install lodash                   # Latest version
npm install lodash@4.17.21           # Exact version
npm install lodash@^4                # Compatible version
npm install -D typescript            # Dev dependency
npm install -D @types/node           # Type definitions
```

---

## 🗑️ Removing Packages

| npm | Yarn |
|-----|------|
| `npm uninstall lodash` | `yarn remove lodash` |
| `npm rm lodash` | `yarn remove lodash` |

```bash
npm uninstall lodash                 # Remove + update package.json
npm uninstall -g typescript          # Remove global package
```

---

## 🔄 Updating Packages

```bash
npm update                           # Update all packages (respects ranges)
npm update lodash                    # Update specific package
npm outdated                         # Show outdated packages

npm install lodash@latest            # Force latest version

# Check what would be updated
npx npm-check-updates                # Show all upgrades available
npx npm-check-updates -u             # Update package.json versions
npm install                          # Install after updating

# Yarn
yarn upgrade                         # Upgrade all packages
yarn upgrade lodash                  # Upgrade specific
yarn upgrade-interactive             # Interactive upgrade picker (Yarn Berry)
```

---

## 🌍 Global Packages

```bash
npm install -g typescript            # Install globally
npm install -g nodemon               # Install nodemon globally
npm uninstall -g typescript          # Remove global package
npm list -g --depth=0                # List global packages
npm root -g                          # Show global node_modules path
npm update -g                        # Update all global packages

# Yarn
yarn global add typescript
yarn global remove typescript
yarn global list
```

---

## ▶️ Scripts

```bash
# Run npm scripts
npm run build                        # Run "build" script
npm run test                         # Run "test" script
npm start                            # Run "start" script (no run needed)
npm test                             # Run "test" script (no run needed)
npm run lint -- --fix                # Pass args to underlying command

# Yarn
yarn build
yarn test
yarn start

# List available scripts
npm run                              # Shows all available scripts
```

---

### Common `package.json` scripts

```json
{
  "scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon src/index.ts",
    "build": "tsc",
    "test": "jest",
    "lint": "eslint src --ext .ts",
    "lint:fix": "eslint src --ext .ts --fix",
    "clean": "rm -rf dist"
  }
}
```

---

## 🔢 Version Management

### Inspect versions

```bash
node -v                              # Node version
npm -v                               # npm version
npm view lodash version              # Latest version of a package
npm view lodash versions             # All versions
npm ls                               # Dependency tree
npm ls lodash                        # Check specific package version
```

---

### Bump package version

```bash
npm version patch                    # 1.0.0 → 1.0.1
npm version minor                    # 1.0.0 → 1.1.0
npm version major                    # 1.0.0 → 2.0.0
npm version prerelease               # 1.0.0 → 1.0.1-0
npm version prepatch --preid=beta    # 1.0.0 → 1.0.1-beta.0
```

> These also create a git tag and commit automatically.

---

### Use specific Node version

```bash
# With nvm
nvm install 20                       # Install Node 20
nvm use 20                           # Use Node 20 in current shell
nvm alias default 20                 # Set default version
nvm ls                               # List installed versions

# With fnm (faster alternative)
fnm install 20
fnm use 20
fnm default 20
```

---

## 📤 Publishing

```bash
npm login                            # Authenticate to npm registry
npm whoami                           # Show logged-in user

npm publish                          # Publish package
npm publish --access public          # Required for scoped packages
npm publish --tag beta               # Publish as beta tag

npm pack                             # Dry run — creates .tgz to inspect
npm pack --dry-run                   # Preview what will be published

npm unpublish my-package@1.0.0       # Remove specific version
npm deprecate my-package@1.0.0 "Use v2 instead"
```

---

## 🧩 Workspaces (Monorepo)

```bash
# npm workspaces (v7+)
npm install --workspaces             # Install all workspace deps
npm install lodash -w packages/app   # Install in specific workspace
npm run build --workspace=packages/app
npm run build --workspaces           # Run in all workspaces
npm ls --workspaces                  # List deps across workspaces

# Yarn workspaces
yarn workspaces info                 # Show workspace tree
yarn workspace packages/app add lodash
yarn workspaces run build            # Run in all workspaces
```

---

## 🔒 Audit & Security

```bash
npm audit                            # Check for vulnerabilities
npm audit --json                     # JSON output
npm audit fix                        # Auto-fix vulnerabilities
npm audit fix --force                # Fix + allow breaking changes

# Yarn
yarn audit
```

> ⚠️ `npm audit fix --force` may break your app. Review changes before deploying.

---

## 💾 Cache & Config

```bash
npm cache clean --force              # Clear npm cache
npm cache verify                     # Verify cache integrity
npm config list                      # Show all config
npm config get registry              # Get registry URL
npm config set registry https://registry.npmjs.org/  # Set registry
npm config set registry https://my.internal.registry/  # Private registry

# Yarn
yarn cache clean                     # Clear yarn cache
yarn cache list                      # List cache entries
yarn config set registry https://registry.npmjs.org/
```

---

### `.npmrc` — common settings

```ini
# .npmrc
registry=https://registry.npmjs.org/
save-exact=true
package-lock=true
```

---

## ⚡ pnpm

> Fast, disk-space-efficient package manager. Uses a content-addressable store (one copy of each version per machine).

### Install pnpm

```bash
npm install -g pnpm
```

### Common pnpm commands

| Action | Command |
|--------|---------|
| Install all dependencies | `pnpm install` |
| Add dependency | `pnpm add lodash` |
| Add dev dependency | `pnpm add -D eslint` |
| Remove package | `pnpm remove lodash` |
| Run script | `pnpm run build` |
| Update packages | `pnpm update` |
| List outdated | `pnpm outdated` |
| Install globally | `pnpm add -g typescript` |
| List global packages | `pnpm list -g` |

```bash
pnpm install                         # Install from package.json
pnpm add lodash                      # Add dependency
pnpm add -D jest                     # Add dev dependency
pnpm add lodash@4.17.21              # Exact version
pnpm remove lodash                   # Remove package
pnpm run dev                         # Run dev script
pnpm dlx create-react-app my-app     # Like npx (without installing)
pnpm store status                    # Check content store
pnpm store prune                     # Clean unused store entries
```

### pnpm workspaces (monorepo)

```bash
# pnpm-workspace.yaml is the config file
pnpm install                         # Install all workspace deps
pnpm add lodash --filter my-app      # Add to specific workspace
pnpm run build --filter my-app       # Run script in specific workspace
pnpm run build --filter "..."        # Run in all workspaces
pnpm ls --filter my-app              # List deps in workspace
```

---

## 🧡 Bun

> All-in-one JavaScript runtime & toolkit: bundler, transpiler, test runner, and package manager.

### Install Bun

```bash
# macOS / Linux
curl -fsSL https://bun.sh/install | bash

# Windows (PowerShell)
powershell -c "irm bun.sh/install.ps1 | iex"
```

### Common Bun commands

| Action | Command |
|--------|---------|
| Install all dependencies | `bun install` |
| Add dependency | `bun add lodash` |
| Add dev dependency | `bun add -d eslint` |
| Remove package | `bun remove lodash` |
| Run script | `bun run build` |
| Run a file directly | `bun run index.ts` |
| Run tests | `bun test` |
| Bundle project | `bun build ./src/index.ts --outdir ./dist` |

```bash
bun install                          # Install from package.json
bun add lodash                       # Add dependency
bun add -d jest                      # Add dev dependency
bun remove lodash                    # Remove package
bun run dev                          # Run dev script
bun x create-next-app my-app         # Like npx
bun upgrade                          # Upgrade bun itself
bun --version                        # Show bun version
```

### Bun test runner

```bash
bun test                             # Run all tests
bun test --watch                     # Watch mode
bun test src/utils.test.ts           # Run specific test file
bun test --coverage                  # With code coverage
```

---

[← Kubernetes](kubernetes.md) | [Back to Home](../README.md) | [🪟 Windows/PowerShell →](windows-powershell.md)
