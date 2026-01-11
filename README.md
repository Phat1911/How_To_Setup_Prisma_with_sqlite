# To set prisma with sqlite in 2026, You need to follow these step in order:

## CMD:
  ```cmd
  mkdir hello-prisma 
  cd hello-prisma
  npm init -y
  npm install typescript tsx @types/node --save-dev
  npx tsc --init
  npx prisma init
  ```

## Tsconfig.json:

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "node",
    "target": "ES2023",
    "strict": true,
    "esModuleInterop": true,
    "ignoreDeprecations": "6.0"
  }
}
```
## Prisma.config.ts:
```ts
import { defineConfig } from 'prisma/config'

export default defineConfig({
  schema: 'prisma/schema.prisma',
  datasource: {
    url: "file:./prisma/dev.db",
  },
})
```

## Package.json:
```json
{
  "name": "prisma-demos",
  "type": "module",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@types/node": "^25.0.3",
    "@types/pg": "^8.16.0",
    "prisma": "^7.2.0"
  },
  "dependencies": {
    "@prisma/adapter-better-sqlite3": "^7.2.0",
    "@prisma/adapter-pg": "^7.2.0",
    "@prisma/client": "^7.2.0",
    "dotenv": "^17.2.3",
    "pg": "^8.16.3"
  },
  "directories": {
    "lib": "lib"
  }
}
```

## Schema.prisma:
```
datasource db {
  provider = "sqlite"
}

generator client {
  provider = "prisma-client-js"
  output   = "../generated/prisma"
}
```
## CMD
```cmd
  npm install prisma @types/node @types/pg --save-dev 
  npm install @prisma/client @prisma/adapter-pg pg dotenv
  npx prisma
```

## src/index.ts
```ts
  import "dotenv/config";
  import { PrismaBetterSqlite3 } from '@prisma/adapter-better-sqlite3';
  import { PrismaClient } from "../generated/prisma/client";
  
  
  const adapter = new PrismaBetterSqlite3({
    url: "file:./prisma/dev.db"
  });
  
  const prisma = new PrismaClient({ adapter });
  
  async function createMovie() {
    const newMovie = await prisma.movie.create({
      data: {
        title: "Inception",
        description:
          "A cinematic masterpiece that seamlessly blends reality and dreams, Inception is a captivating story of a dream within a dream.",
        genre: "Sci-Fi",
        releaseDate: new Date("2010-07-16"),
        rating: 8.8,
      },
    });
  
    console.log(newMovie);
  }
  
  async function main () {
  
  }
  
  main()
  .then(async() => await prisma.$disconnect())
  .catch(async(e) => {
    console.log(e);
    await prisma.$disconnect();
    process.exit(1);
  })
```

## CMD
```cmd
  npx prisma migrate dev --name init
  npx prisma generate
  npx tsx index.ts
  npx prisma studio
```
