# require-from-esm

Forked from [jaydson/require-from-esm](https://github.com/jaydson/require-from-esm) to tweak to make it work with my setup (Node 18 with [ts-node](https://www.npmjs.com/package/ts-node)).

This is my working `yarn start` config:
```
  "scripts": {
    "start": "ts-node --esm app.ts"
  },
```

My `tsconfig.json`:
```
{
	"compilerOptions": {
		"baseUrl": ".",
		"module": "esnext",
		"target": "esnext",
		"moduleResolution": "nodenext",
		"allowImportingTsExtensions": true,
		"esModuleInterop": true,
		"allowJs": true,
		"allowSyntheticDefaultImports": true,
		"declaration": true,
		"isolatedModules": true,
		"resolveJsonModule": true,
		"incremental": true
	},
	"include": ["**/*.ts"],
	"exclude": ["node_modules"]
}
```

And  the fix for the module I was having trouble with:
```
import require from 'require-from-esm';
const expressJSDocSwagger = require('express-jsdoc-swagger');
```

Below is the original README with some minor edits from me, e.g., I found I did not need the `--experimental-modules` flag.
---

⚠️ Warning note ⚠ ️

1) This is very experimental
2) Node.js'12.2.0 or later is required
3) You may need to run Node.js with the `--experimental-modules` flag
4) You may need to run Node.js with the `--es-module-specifier-resolution=node` flag

`require-from-esm` is a small package with literally few lines aiming to provide interoperability between CJS modules and ES modules.  
To be more specific, `require-from-esm` let you require CJS modules with the old friend Node.js global `require()` from ES modules.  
With ES modules on Node.js we can use almost every feature from ECMA modules (still experimental).

Example:
```
import require from 'require-from-esm';
const mymodule = require('../../mymodule.cjs'); // You should use `../../` for local modules *
```

For local modules you should use `require(../../MYMODULE)` instead of `require(./MYMODULE)`.  
We don't have `__dirname` and `__pathname` globals when using ES modules.  
No solution was found to fix it until now.
PRs are welcome.

## How to use
1) Be sure you're using Node.js >12.2.0
2) The flags, you may need the flags: `--experimental-modules` and `--es-module-specifier-resolution=node`
3) You can use npm scripts to run your app, something like this:
```
  "scripts": {
    "start": "node --experimental-modules --es-module-specifier-resolution=node index.js"
  },
```
4) Make sure you have the `"type": "module"` on your package.json.
