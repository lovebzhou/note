https://docs.nestjs.com/cli/overview

## Start

```sh
# 通过npx执行nest
npx nest new project-name

# 本地安装
npm install -g @nestjs/cli
nest new project-name

# 及时更新
npm update -g @nestjs/cli
```

## Generate a Nest element

### Controller

```sh
nest g controller Cats
CREATE src/cats/cats.controller.spec.ts (478 bytes)
CREATE src/cats/cats.controller.ts (97 bytes)
UPDATE src/app.module.ts (322 bytes)
```
