# Build a Package based on `typescript`

## Create a git repository

```bash
git init
echo "# your project introduction" >> README.md
git add . && git commit -m "first"
git remote add [remoteName] [url]
git checkout -b [branchName]
git push [remoteName] [branchName]/master
echo "/node_modules" >> .gitignore
```

## Init your Package

```bash
npm init -y
```

Add Typescript as a DevDependency

```bash
npm install --save-dev typescript
echo "" >> tsconfig.json
```

Edit the tsconfig.json to compile typescript:

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "CommonJS",
    "declaration": true,
    "strict": true,
    "outDir": "./lib"
  },
  "include": ["src"],
  "exclude": ["node-modules", "**/__tests__/*"]
}
```

Modify the .gitignore to ignore the built code:

```sh
/lib
```

Modify the package.json:

```json
{
  "scripts": {
    "build": "tsc"
  }
}
```

Now run the build command:

```bash
npm run build
```

## Formatting

Install Prettier to format your code

```bash
npm install --save-dev prettier
echo "" >> .prettierrc
```

Edit the .prettierrc:

```json
{
  "printWidth": 120,
  "trailingComma": "all",
  "singleQuote": true
}
```

Modify the package.json:

```json
{
  "scripts": {
    "format": "prettier --write \"src/**/*.ts\""
  }
}
```

Now run the format command:

```bash
npm run format
```

# Linting

Install Eslint and its friends

```bash
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
echo "" >> eslintrc.js
echo "" >> .eslintignore
```

Edit the eslintrc.js:

```js
module.exports = {
  root: true,
  parser: "@typescript-eslint/parser",
  plugins: ["@typescript-eslint"],
  extends: ["eslint:recommended", "plugin:@typescript-eslint/recommended"],
  env: {
    browser: true,
    es6: true,
    jest: true,
  },
};
```

Edit the .eslintignore:

```sh
.eslintrc.js
# don't ever lint node_modules
node_modules

# don't lint build output (make sure it's set to your correct build folder name)
lib
```

Modify the package.json:

```json
{
  "scripts": {
    "eslint": "eslint . --ext .js,.jsx,.ts,.tsx"
  }
}
```

Now run the format command:

```bash
npm run format
```

You can also get results in realtime inside most IDEs via a plugin - search your IDE's extension store such as `ESLint` in vscode.
