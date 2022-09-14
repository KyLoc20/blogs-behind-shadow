# Build a Typescript Library (1): Get Started

This part talks about using `tsc`, `tsconfig.json`, `npm` and `package.json` to build a 101 Typescript Library(a React UI Component) for other projects to utilize. For more details see [How to Write a TypeScript Library](https://www.tsmean.com/articles/how-to-write-a-typescript-library/).

## Step1: Setup tsconfig.json

```json
{
    "compilerOptions": {
        "target": "ES5", 
        "module": "esnext", 
        "declaration": true, 
        "outDir": "./build"
    },
    "include": ["src/**/*"],
    "exclude": ["node-modules", "**/__tests__/*"]
}
```

This is a basic setting, explain some options from `compilerOptions`:
- `target` specifies which version of JavaScript your code will get transpiled to, usually `ES5` or `ES6`(equals to `ES2015`)
- `module` specifies which module solution, usually `ESNext`(for modern browsers & nodejs) or `CommonJS`(for nodejs, if this lib doesn't in browsers, choose this), see more details in [The TypeScript Module Compiler Option](https://www.tsmean.com/articles/learn-typescript/typescript-module-compiler-option/).
- `declaration: true` generates `*.d.ts` files, if someone is using your library and they also use TypeScript, they get the benefits of typesafety and autocomplete.

Plus, there are following options to specify more details:

```json
{
    "compilerOptions": {
        "jsx": "react", 
        "moduleResolution":"Node", 
        "allowSyntheticDefaultImports":true 
    },
}
```

- `jsx` enables JSX.
- `moduleResolution` helps resolve module paths, set `"Node"` If you are having resolution problems with imports and exports in TypeScript, see more details in [module-resolution](https://www.typescriptlang.org/docs/handbook/module-resolution.html).
- `allowSyntheticDefaultImports` enables default import. 

## Step2: Setup package.json

```json
{
    "name": "@who/what",
    "version": "0.1.0",
    "description": "xxxxx",
    "main": "build/index.js",
    "types": "build/index.d.ts",
    "files": [
        "/build"
    ],
    "scripts": {
        "test": "jest",
        "build": "rimraf build && rimraf types && tsc"
    },
    "peerDependencies": {
        "react": ">=17",
        "react-dom": ">=17"
    },
    "devDependencies": {
        "@types/jest": "^27.4.0", //npm install --save-dev jest
        "@types/react": ">=17",
        "@types/react-dom": ">=17",
        "jest": "^27.4.5", //npm install --save-dev jest
        "rimraf": "^3.0.2", //npm install --save-dev rimraf
    },
    "keywords": [],
    "author": "your email",
    "license": "ISC"
}
```

This is a basic setting:
- `main` defines entrypoint of the package as a module.
- `types` declares where to find the type declarations otherwise the consumer won't find your module.
- `files` whitelists the files you wanna ship to the npm registry.
- `peerDependencies` helps to organize dependencies of a module well, see more details in [Peer Dependencies](https://nodejs.org/es/blog/npm/peer-dependencies/)
 
## Step3: Write your library

Here is a styled TextField React UI component(now it's "unstyled" because we handle CSS and styles in next part) for example:

```tsx
// src/index.tsx
import React, { PropsWithChildren, useState, ChangeEvent } from "react";

type TextFieldProps = {
  onChange?: (value: string) => void;
  defaultValue?: string;
  placeholder?: string;
};

export default function TextField(props: PropsWithChildren<TextFieldProps>) {
  const { onChange, placeholder, defaultValue } = props;
  const [isFocused, setIsFocused] = useState(false);

  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    onChange?.(e.target.value);
  };

  return (
    <label className="mymui-textfield">
      <input
        placeholder={placeholder ?? ""}
        onChange={handleChange}
        onFocus={() => setIsFocused(true)}
        onBlur={() => setIsFocused(false)}
        defaultValue={defaultValue}
      />
    </label>
  );
}
```

Let's transpile all .ts/tsx files in `./src` into .js file in `./build`:

```bash  
npm run build
```

## Step4: Test your library locally

In another project, use `npm link`:

```bash
npm link [path like ../myLib or other spec]
```

Consume your lib:

```tsx
import { someThing } from 'nameDefinedInPackageJson'
```

When you wanna cancel this link: 

```bash
npm unlink [path like ../myLib or other spec]
```

## Step5: Publish to npm

```bash
npm run build
npm adduser
npm publish --access=public
```

---

Other references:
- [package-lock.json](https://dev.to/adamklein/package-lock-json-in-git-or-not-50l5)
 