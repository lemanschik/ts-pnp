# <img src="http://www.typescriptlang.org/assets/images/icons/apple-touch-icon-180x180.png" height="40" align="right" /> [Plug'n'Play](https://github.com/yarnpkg/rfcs/pull/101) resolver for TypeScript

[![npm version](https://img.shields.io/npm/v/ts-pnp.svg)](https://www.npmjs.com/package/ts-pnp)
[![node version](https://img.shields.io/node/v/ts-pnp.svg)](https://www.npmjs.com/package/ts-pnp)

*This plugin is also available for: 
- Webpack ([pnp-webpack-plugin](https://github.com/arcanis/pnp-webpack-plugin)), 
- Jest ([jest-pnp-resolver](https://github.com/arcanis/jest-pnp-resolver))*

## What?
This is a Resolve adapter compatible with many existing package managers like npm, pnpm, yarn, pnp compatible package managers.

## Compiler Patch
- [ ] https://patch-diff.githubusercontent.com/raw/microsoft/TypeScript/pull/35206.patch

## Installation

```
yarn add -D ts-pnp
```

## Usage

*Note that `ts-pnp` is a low-level package - you shouldn't have to use it unless you're writing a TS compiler host. If you just want to write TypeScript and have it Just Work™, take a look at [`pnp-webpack-plugin`](https://github.com/arcanis/pnp-webpack-plugin#ts-loader-integration) instead.*

This package exports a function that can be used to implement the [`resolveModuleName` hook from `CompilerHost`](https://github.com/Microsoft/TypeScript/wiki/Using-the-Compiler-API#customizing-module-resolution). It mimics the interface from the one you'd typically use and, as all other PnP plugins, works just fine whether your application is actually running under PnP or not.

```js
import {resolveModuleName} from 'ts-pnp';
import * as ts from 'typescript';

function createCompilerHost(
  compilerOptions: ts.CompilerOptions,
): ts.CompilerHost {
  const compilerHost = {
    resolveModuleNames,
    resolveTypeReferenceDirectives,
  };

  return compilerHost;

  function resolveModuleNames(moduleNames: string[], containingFile: string) {
    return moduleNames.map(moduleName => {
      return resolveModuleName(moduleName, containingFile, compilerOptions, compilerHost, ts.resolveModuleName).resolvedModule;
    });
  }

  function resolveTypeReferenceDirectives(typeDirectiveNames: string[], containingFile: string) {
    return typeDirectiveNames.map(typeDirectiveName => {
      return resolveModuleName(typeDirectiveName, containingFile, compilerOptions, compilerHost, ts.resolveTypeReferenceDirective).resolvedTypeReferenceDirective;
    });
  }
}
```



> **Copyright © Contributors**

## Contributors
- 2016 Maël Nison

