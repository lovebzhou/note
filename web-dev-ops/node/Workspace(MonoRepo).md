## 1. 概述
### 参考资料
- https://lerna.js.org/docs/getting-started
- https://pnpm.io/workspaces
- https://docs.npmjs.com/cli/v8/using-npm/workspaces
- https://yarnpkg.com/features/workspaces
## 2. Tools

### `lerna`
```sh
# =>>> 初始化

npx lerna init --dryRun

# =>>> 新建package
lerna create package-name -y

# =>>> 构建

# 构建全部
npx lerna run build

# 构建指定package
npx lerna run build --scope=package-name
```

### `npm`
```sh
# 新建package-a
npm init -w ./packages/package-a

# 为package-a添加依赖
npm install lodash -w package-a
```
### `pnpm`

#### `pnpm-workplace.yaml`
pnpm-workspace.yaml 定义工作区的根目录，并允许您在工作区中包含/排除目录。默认情况下，包含所有子目录的所有包。[更多...](https://pnpm.io/pnpm-workspace_yaml)
```yaml
packages:
  # all packages in direct subdirs of packages/
  - 'packages/*'
  # all packages in subdirs of components/
  - 'components/**'
  # exclude packages that are inside test directories
  - '!**/test/**'
```
