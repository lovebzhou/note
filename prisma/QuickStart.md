https://www.prisma.io/docs/getting-started/quickstart

## 1. Create TypeScript project and set up Prisma

```Shell
makedir hello
cd hello

npm init -y
npm install typescript ts-node @types/node --save-dev

npx tsc --init

# 安装Prisma
npm install prisma -D

# 使用init命令安装Prisma数据库：创建prisma目录
npx prisma init --datasource-provider sqlite
```

## 2. 在Prisma schema中建模数据
Add the following models to your `./prisma/schema.prisma` file:
``` JavaScript
model User {
	id Int @id @default(autoincrement())
	email String @unique
	name String?
	posts Post[]
}

model Post {
	id Int @id @default(autoincrement())
	title String
	content String?
	published Boolean @default(false)
	author User @relation(fields: [authorId], references: [id])
	authorId Int
}
```
Models in the Prisma schema have two main purposes:

-   Represent the tables in the underlying database
-   Serve as foundation for the generated Prisma Client API
## 3. Run a migration to create your database tables with Prisma Migrate
```Shell
npx prisma migrate dev --name init
```

## 4. Explore how to send queries to your database with Prisma Client

```TypeScript
import {PrismaClient} from '@prisma/client'

const prisma = new PrismaClient()

async function main() {
	const user = await prisma.user.create({
		data: {
			name: 'Alice',
			email: 'alice@prisma.io',
		},
	})
	console.log(user)
}

  

main()
	.then(async () => {
		await prisma.$disconnect()
	})
	.catch(async e => {
		console.error(e)
		await prisma.$disconnect()
		process.exit(1)
	})
```



```Shell
# 运行
npx ts-node script.ts


```

## Explore the data in Prisma Studio

```Shell
npx prisma studio
```