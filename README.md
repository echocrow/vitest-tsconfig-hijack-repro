## Setup

- Install dependencies via `pnpm i`.
- Ensure VSCode is running.
- Ensure VSCode "Vitest" extension (`vitest.explorer`) is enabled.

## Repro

Reset: Build SvelteKit and log its internal `tsconfig.json`.

```sh
pnpm run reset
```

- Expected & actual output (after some build logs):

```json
{
  "paths": {
    "myAlias": ["../src/myAlias"],
    "myAlias/*": ["../src/myAlias/*"],
    "$lib": ["../src/lib"],
    "$lib/*": ["../src/lib/*"]
  },
  "rootDirs": ["..", "./types"]
}
```

Hijack: Touch the root package.json, wait a few seconds, and log SvelteKit's internal `tsconfig.json` again.

```sh
pnpm run hijack
```

- Expected output: _(same as above)_
- Actual output:

```json
{
  "paths": {
    "myAlias": ["../../z-another-app/src/myAlias"],
    "myAlias/*": ["../../z-another-app/src/myAlias/*"],
    "$lib": ["../src/lib"],
    "$lib/*": ["../src/lib/*"]
  },
  "rootDirs": ["../../z-another-app", "./types"]
}
```
