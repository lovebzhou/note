## 概述
https://nestjs.com/
- [Nest.js 的微服务，写起来也太简单了吧！](https://juejin.cn/post/7207637337571901495)
- [一文学会用 Docker 和 Docker Compose 部署 Node.js 微服务](https://juejin.cn/post/7208384641190723644)


## 2. QuickStart

### 安装
```sh
npm i -g @nestjs/cli
```

### 新建

```sh
nest new hello-nest

# 选择yarn

# 启动
yarn start
```

## 3. 创建微服务

### 计算器微服务

#### 新建 & 运行
```sh
nest new micro-calc

# 包管理工具选择npm
npm i @nestjs/microservices

# 调试运行
npm run start:debug
```

#### 修改：main.ts
```typescript
import { NestFactory } from '@nestjs/core';
import { Transport, MicroserviceOptions } from '@nestjs/microservices';
import { AppModule } from './app.module';

async function bootstrap() {
	const app = await NestFactory.createMicroservice<MicroserviceOptions>(
		AppModule,
		{
			transport: Transport.TCP,
			options: {
				port: 8888,
			},
		},
	);
	app.listen();
}
bootstrap();
```
#### 修改：app.controller.ts
```TypeScript
import { Controller } from '@nestjs/common';
import { MessagePattern } from '@nestjs/microservices';

@Controller()
export class AppController {
	// constructor() {}
	
	@MessagePattern('sum')
	sum(numArr: Array<number>): number {
		return numArr.reduce((total, item) => total + item, 0);
	}
}
```

### 日志微服务

#### 新建 & 运行
```sh
nest new micro-log

# 包管理工具选择npm
npm i @nestjs/microservices

# 调试运行
npm run start:debug
```

#### 修改：main.ts
```typescript
import { NestFactory } from '@nestjs/core';
import { Transport, MicroserviceOptions } from '@nestjs/microservices';
import { AppModule } from './app.module';

async function bootstrap() {
	const app = await NestFactory.createMicroservice<MicroserviceOptions>(
		AppModule,
		{
			transport: Transport.TCP,
			options: {
				port: 9800,
			},
		},
	);
	app.listen();
}
bootstrap();
```
#### 修改：app.controller.ts
```TypeScript
import { Controller } from '@nestjs/common';
import { MessagePattern } from '@nestjs/microservices';

@Controller()
export class AppController {
	// constructor() {}
	
	@MessagePattern('')
	sum(text: string): void {
		console.log(text);
	}
}
```

## 3. 使用微服务

### 新建 & 运行
```sh
nest new hello-micro

# 包管理工具选择npm
npm i @nestjs/microservices

# 
npm i rxjs

# 调试运行
npm run start:debug
```
### 代码修改

#### 修改：app.controller.ts
```TypeScript
import { Controller, Get, Inject, Query } from '@nestjs/common';
import { ClientProxy } from '@nestjs/microservices';
import { Observable } from 'rxjs';

@Controller()
export class AppController {
	constructor(
		@Inject('CALC_SERVICE') private calcClient: ClientProxy,
		@Inject('LOG_SERVICE') private logClient: ClientProxy,
	) {}
	
	@Get()
	calc(@Query('num') str): Observable<number> {
		const numArr = str.split(',').map((item) => parseInt(item));
		
		this.logClient.emit('log', '#calc#: ' + numArr);
		
		return this.calcClient.send('sum', numArr);
	}
	
	@Get()
	getHello(): string {
		return 'Hello World!';
	}
}
```

#### 修改：app.module.ts
```TypeScript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { ClientsModule, Transport } from '@nestjs/microservices';

@Module({
	imports: [
		ClientsModule.register([
			{
				name: 'CALC_SERVICE',
				transport: Transport.TCP,
				options: {
					port: 8888,
				},
			},
		]),
		ClientsModule.register([
			{
				name: 'LOG_SERVICE',
				transport: Transport.TCP,
				options: {
					port: 9800,
				},
			},
		]),
	],
	controllers: [AppController],
	providers: [],

})

export class AppModule {}
```

### 演示

在浏览器中打开`http://localhost:3000/?num=1,2,3,4,5`，输出：`15`

在micro-log控制台输出：`#calc#: 1,2,3,4,5`


## 4. Docker部署

### Dockerfile

```Dockerfile
FROM node:alpine As development

WORKDIR /usr/app

COPY package.json ./

RUN npm install

COPY . .

RUN npm run build

FROM node:alpine as production

WORKDIR /usr/app

COPY package.json ./

RUN npm install --only=production

COPY . .

COPY --from=development /usr/app/dist ./dist

CMD ["node", "dist/main.js"]
```

### docker-compose.yml

```yaml
services:
  main-app:
    build:
      context: ./main-app
      dockerfile: ./Dockerfile
    depends_on:
      - ms-calc
      - ms-log
      - rabbitmq
    ports:
      - '9800:9800'
  ms-calc:
    build:
      context: ./micro-service-calc
      dockerfile: ./Dockerfile
    ports:
      - '9801:9801'
  ms-log:
    build:
      context: ./micro-service-log
      dockerfile: ./Dockerfile
    ports:
      - '9802:9802'
```

### 运行

```bash
# 构建镜像
docker build -t main-app .

# 运行
docker run -p 6000:6000 main-app

# docker-compose up
docker-compose up
```