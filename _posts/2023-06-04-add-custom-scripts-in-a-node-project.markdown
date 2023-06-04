---
layout: post
title: "Add custom scripts in a Node project"
date: 2023-06-10 19:22:00 +0200
tags: javascript node.js development-experience
categories: software-development
---

One of many things I like about [Rails](https://rubyonrails.org/) is it's CLI. Scaffolding, generating migrations, creating various types of resources for the application and other stuff is so neat and it's no wonder many frameworks copied it shamelessly. 

Once project grows beyond certain point, doing routine tasks becomes tedious. When this happens, I write scripts to automate various jobs. It really helps in maintaining productivity and focusing on important stuff.

Here is a quick way how to do this on any Node project if your framework of choice doesn't already provide some way to write custom commands for the CLI. After we're finished you would be able to do something like this:

```
npm run script generate migration <migration_name>
```

1. Create shell script which will serve as an entry point. I typically put it in `src/scripts` along with the modules of the scripts I'm going to create. Let's call it `entry.sh`. Don't forget to allow execution of this script with `chmod`.

```sh
#!/usr/bin/env bash
/usr/bin/env node --no-warnings --experimental-specifier-resolution=node ./src/scripts/entry.js "$@"
```

2. In your `package.json` add NPM script which will be used for running our custom scripts.

```json
{
  "scripts": {
    "script": "./src/scripts/entry.sh" // our NPM script
  },
}
```

3. Create `entry.js` file which will serve as the processor for arguments we'll pass.

```js
import { createMigration } from './migrationManager';

// First two arguments are path to the Node executable and path of this file respectively; let's just ditch them
const usefulArguments = process.argv.slice(2);
const [first, second, ...rest] = usefulArguments;

if (['g', 'generate'].includes(first)) {
  if (second === 'migration') {
    createMigration(...rest);
  }
}
```

I'm keeping things very simple intentionally; once your list of scripts grows you'd probably like to use some arguments parser for better organization and documentation (e.g. [commander](https://www.npmjs.com/package/commander), [yargs](https://www.npmjs.com/package/yargs), [argparse](https://www.npmjs.com/package/argparse)...).

## Support for TypeScript and path aliases

If you've been concerned about the `.js` extension because your project is using TypeScript don't worry - there is a way to do this with support for TypeScript **and** [path aliases](https://www.typescriptlang.org/tsconfig#paths) (if you're using them in your project).

1. We'll need to install few development dependencies before we continue with the process.

```
npm install ts-node tsconfig-paths -D
```

2. Create a `scriptsLoader.js` file in the root of your project. This will ensure that your scripts written in TypeScript could be executed and that paths using aliases defined in `tsconfig.json` will be properly resolved.

```js
import { resolve as resolveTs } from 'ts-node/esm';
import * as tsConfigPaths from 'tsconfig-paths';
import { pathToFileURL } from 'url';

const { absoluteBaseUrl, paths } = tsConfigPaths.loadConfig();
const matchPath = tsConfigPaths.createMatchPath(absoluteBaseUrl, paths);

export function resolve(specifier, ctx, defaultResolve) {
  const match = matchPath(specifier);
  return match
    ? resolveTs(pathToFileURL(`${match}`).href, ctx, defaultResolve)
    : resolveTs(specifier, ctx, defaultResolve);
}

export { load, transformSource } from 'ts-node/esm';
```

3. Update NPM script so we can pass path to the loader we've just made.

```json
{
  "scripts": {
    "script": "./src/scripts/entry.sh ./scriptsLoader.js"
  }
}
```

4. Update `src/scripts/entry.sh` to pass path to the loader.

```sh
#!/usr/bin/env bash

all_args=("$@")
loader_path="$1"
rest_args=("${all_args[@]:1}")

/usr/bin/env node --no-warnings --experimental-specifier-resolution=node --loader "$loader_path" ./src/scripts/entry.ts "${rest_args[@]}"
```

And that's it!
