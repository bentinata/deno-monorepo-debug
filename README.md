# Repo Structure

- `apps/vite-deno-react` created with `deno init --npm vite`
- `apps/vite-extra-deno-react` created with `deno init --npm vite-extra`

# Observations

`deno task dev` from `apps/vite-deno-react` would succeed either way, while `deno task dev` from `apps/vite-extra-deno-react` behave differently before and after `npm install` on root repository. This is the output of `deno task dev` after `npm install`:

```
$ deno task dev
Task dev deno run -A --node-modules-dir npm:vite
error: Uncaught (in promise) ReferenceError: require is not defined
const os = require('os');
           ^
    at file:///Users/bentinata/deno-monorepo-debug/node_modules/supports-color/index.js:2:12
    at loadESMFromCJS (node:module:777:21)
    at Module._compile (node:module:722:12)
    at loadMaybeCjs (node:module:770:10)
    at Object.Module._extensions..js (node:module:755:12)
    at Module.load (node:module:662:32)
    at Function.Module._load (node:module:534:12)
    at Module.require (node:module:681:19)
    at require (node:module:812:16)
    at file:///Users/bentinata/deno-monorepo-debug/apps/vite-extra-deno-react/node_modules/.deno/vite@5.4.11/node_modules/vite/dist/node/chunks/dep-CB_7IfJ-.js:16240:26

    info: Deno supports CommonJS modules in .cjs files, or when the closest
          package.json has a "type": "commonjs" option.
    hint: Rewrite this module to ESM,
          or change the file extension to .cjs,
          or add package.json next to the file with "type": "commonjs" option.
    docs: https://docs.deno.com/go/commonjs
```

Copying `apps/vite-deno-react`, running `deno run -A npm:vite` from `apps/vite-extra-deno-react` and omitting `--node-modules-dir` would result in different errors:

```
$ deno run -A npm:vite
failed to load config from /Users/bentinata/deno-monorepo-debug/apps/vite-extra-deno-react/vite.config.ts
error when starting dev server:
TypeError: Relative import path "assert" not prefixed with / or ./ or ../ and not in import map from "file:///Users/bentinata/deno-monorepo-debug/apps/vite-extra-deno-react/node_modules/.deno/vite@5.4.11/node_modules/vite/dist/node/index.js"
  hint: If you want to use a built-in Node module, add a "node:" prefix (ex. "node:assert").
    at file:///Users/bentinata/deno-monorepo-debug/apps/vite-extra-deno-react/node_modules/.deno/vite@5.4.11/node_modules/vite/dist/node/index.js:46:8
    at async loadConfigFromBundledFile (file:///Users/bentinata/Library/Caches/deno/npm/registry.npmjs.org/vite/5.4.11/dist/node/chunks/dep-CB_7IfJ-.js:66691:15)
    at async loadConfigFromFile (file:///Users/bentinata/Library/Caches/deno/npm/registry.npmjs.org/vite/5.4.11/dist/node/chunks/dep-CB_7IfJ-.js:66532:24)
    at async resolveConfig (file:///Users/bentinata/Library/Caches/deno/npm/registry.npmjs.org/vite/5.4.11/dist/node/chunks/dep-CB_7IfJ-.js:66140:24)
    at async _createServer (file:///Users/bentinata/Library/Caches/deno/npm/registry.npmjs.org/vite/5.4.11/dist/node/chunks/dep-CB_7IfJ-.js:62758:18)
    at async CAC.<anonymous> (file:///Users/bentinata/Library/Caches/deno/npm/registry.npmjs.org/vite/5.4.11/dist/node/cli.js:735:20)
```

Again, copying `apps/vite-deno-react`, creating `package.json` that contains `{"type":"module"}` would make `deno run -A npm:vite` succeed.

Upgrading from `storybook@7` to `storybook@8` make `vite-extra` issues gone. Try it on the `storybook-8` branch.
