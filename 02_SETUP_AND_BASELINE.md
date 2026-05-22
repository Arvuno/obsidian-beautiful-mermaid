# 02 SETUP AND BASELINE — obsidian-beautiful-mermaid

## Package Scripts (package.json)

| Script | Command |
|--------|---------|
| `build` | `tsc --noEmit --skipLibCheck && node esbuild.config.mjs --production` |
| `dev` | `node esbuild.config.mjs` |
| `typecheck` | `tsc --noEmit --skipLibCheck` |
| `assets` | `bun scripts/generate-assets.ts` |

## Install Results

- **npm install --legacy-peer-deps** — succeeded (peer dep conflict with `obsidian` devDependency; resolved with `--legacy-peer-deps`)
- **Output**: 1 moderate severity vulnerability reported (not fixed)

## Build Results

### TypeScript Check
```
npx tsc --noEmit --skipLibCheck → PASSED (no errors)
```

### esbuild
```
node esbuild.config.mjs --production → PASSED
Output: main.js (~3.5MB bundled)
```

## Test Script
- No test script defined in package.json

## Type Checking
- `npm run typecheck` maps to `tsc --noEmit --skipLibCheck`
- TypeScript strict mode enabled (tsconfig.json)
- Type check passes cleanly

## Dependency Health
- 1 moderate severity vulnerability in transitive deps (not addressed)
- Peer dependency conflict with `obsidian@^1.8.7` in devDependencies requires `--legacy-peer-deps`

## manifest.json

```json
{
  "id": "beautiful-mermaid-renderer",
  "name": "Beautiful Mermaid Renderer",
  "version": "0.2.3",
  "minAppVersion": "1.5.0",
  "description": "Render Mermaid diagrams with beautiful-mermaid SVG output and theme variables.",
  "author": "qiaoborui",
  "authorUrl": "https://github.com/qiaoborui",
  "isDesktopOnly": false
}
```

All required fields present: ✅ id, name, version, minAppVersion, description, author