## 1. 概述

### 参考资料
- [How to Create and Publish React TypeScript npm Package With Demo and Automated Build](https://betterprogramming.pub/how-to-create-and-publish-react-typescript-npm-package-with-demo-and-automated-build-80c40ec28aca)
- [Create a React component library with Vite and Typescript](https://dev.to/nicolaserny/create-a-react-component-library-with-vite-and-typescript-1ih9)

## 2. Quick Start

### 2.1. 准备

```sh
# =>>> Prepare

mkdir hello-package
cd hello-package

# create package.json
npm init -y

# 创建src文件夹
mkdir src

# Add React and TypeScript to the project
yarn add -D react react-dom typescript @types/react
```
### 2.2. 组件编写

### 2.3. 编译

#### typescript配置：`tsconfig.json`

```json
{
  "include": ["src"],
  "exclude": [
    "dist",
    "node_modules"
  ],
  "compilerOptions": {
    "module": "esnext",
    "lib": ["dom", "esnext"],
    "importHelpers": true,
    "declaration": true,
    "sourceMap": true,
    "rootDir": "./src",
    "outDir": "./dist/esm",
    "strict": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "moduleResolution": "node",
    "jsx": "react",
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
  }
}
```
#### rollup编译：`rollup.config.js`

```js
import typescript from 'rollup-plugin-typescript2'

export default {
  input: ['src/index.tsx'],
  output: [
    {
      dir: 'dist',
      entryFileNames: '[name].js',
      format: 'esm',
      exports: 'named',
      sourcemap: true,
    },
  ],
  plugins: [typescript()],
  external: ['react'],
}
```

#### 增加构建脚本：`package.json`

```json
{
  "name": "my-react-typescript-package",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build:tsc": "tsc",
    "build:rollup": "rollup --config"
  },
  "keywords": [],
  "author": "gapon2401",
  "license": "ISC",
  "devDependencies": {
    "@types/react": "^18.0.12",
    "react": "^18.1.0",
    "react-dom": "^18.1.0",
    "typescript": "^4.7.3"
  }
}
```

#### 构建
```sh
yarn build
npm build
pnpm build
```

### 2.4. ESLint

```sh
yarn add -D eslint eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/eslint-plugin @typescript-eslint/parser
```
- `.eslintrc`
```json
{
  "root": true,
  "extends": [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "plugins": [
    "@typescript-eslint",
    "react",
    "react-hooks"
  ],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "@typescript-eslint/no-non-null-assertion": "off",
    "@typescript-eslint/ban-ts-comment": "off",
    "@typescript-eslint/no-explicit-any": "off"
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "env": {
    "browser": true,
    "node": true
  },
  "globals": {
    "JSX": true
  }
}
```
- `.eslintignore`
```
node_modules  
dist
```
- `package.json`增加lint脚本
```json
"lint": "eslint \"{**/*,*}.{js,ts,jsx,tsx}\""
```

### 2.5. Prettier

```sh
yarn add -D eslint-config-prettier eslint-plugin-prettier prettier
```
- `.prettierrc.json`
```json
{
  "bracketSpacing": false,
  "singleQuote": true,
  "trailingComma": "all",
  "arrowParens": "avoid",
  "tabWidth": 2,
  "semi": false,
  "printWidth": 120,
  "jsxSingleQuote": false,
  "endOfLine": "auto"
}
```
- add `prettier` to `.eslintrc`
```
{
  "root": true,
  "extends": [
    "prettier",
    "plugin:prettier/recommended",
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "prettier", "react", "react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    "@typescript-eslint/no-non-null-assertion": "off",
    "@typescript-eslint/ban-ts-comment": "off",
    "@typescript-eslint/no-explicit-any": "off"
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  },
  "env": {
    "browser": true,
    "node": true
  },
  "globals": {
    "JSX": true
  }
}
```
- 添加脚本到`package.json`中
```json
"prettier": "prettier --write \"{src,tests,example/src}/**/*.{js,ts,jsx,tsx}\""
```
