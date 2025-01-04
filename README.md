# ts-node paths issue

This repository showcases an issue in running a simple TS app using tsconfig `paths` configuration, with `ts-node`.

```
PS C:\src\sample-paths\app\function> npm run start

> function@1.0.0 start
> ts-node index.ts

Error: Cannot find module 'entities/SimpleClass'
Require stack:
- C:\src\sample-paths\app\function\index.ts
    at Function.Module._resolveFilename (node:internal/modules/cjs/loader:1225:15)
    at Function.Module._resolveFilename.sharedData.moduleResolveFilenameHook.installedValue (C:\src\sample-paths\app\function\node_modules\@cspotcode\source-map-support\source-map-support.js:811:30)
    at Function.Module._resolveFilename (C:\src\sample-paths\node_modules\tsconfig-paths\src\register.ts:115:36)
    at Function.Module._load (node:internal/modules/cjs/loader:1051:27)
    at Module.require (node:internal/modules/cjs/loader:1311:19)
    at require (node:internal/modules/helpers:179:18)
    at Object.<anonymous> (C:\src\sample-paths\app\function\index.ts:1:1)
    at Module._compile (node:internal/modules/cjs/loader:1469:14)
    at Module.m._compile (C:\src\sample-paths\app\function\node_modules\ts-node\src\index.ts:1618:23)
    at Module._extensions..js (node:internal/modules/cjs/loader:1548:10) {
  code: 'MODULE_NOT_FOUND',
  requireStack: [ 'C:\\src\\sample-paths\\app\\function\\index.ts' ]
}
```

Changes that make it work:
* Set `baseUrl` to `.` in `tsconfig.base.json`
* Duplicate the `paths` in `app/function/tsconfig.json`:

```json
"paths": {
  "entities/*": ["../entities/*"],
}
```

## Reproduction

1. Clone the repo
2. Run `npm install` in the root and in `app/function`
3. Run `npm run start` in `app/function`

There are a couple of logs available with the different configurations:
* [`not-working.txt`](./logs/not-working.txt) - the default configuration
* [`base-url.txt`](./logs/base-url.txt) - the configuration using `baseUrl` in `tsconfig.base.json`
* [`duplicate.txt`](./logs/duplicate.txt) - the configuration duplicating the `paths` in `app/function/tsconfig.json`
