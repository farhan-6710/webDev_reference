# Server (Express + TypeScript)

Minimal Express server written in TypeScript.

## Run

```bash
cd server
npm install
```
# Server (Express + TypeScript)

Minimal Express server written in TypeScript.

## Run

```bash
cd server
npm install
```

Development (nodemon + ts-node):

```bash
npm run dev
```

Production build + start:

```bash
npm run build
npm start
```

## Environment

Create `server/.env` if needed:

```env
PORT=5000
NODE_ENV=development
```

## What’s implemented

- `GET /` → returns a simple “server running just fine” response
- `GET /api/v1/signup` → demo signup handler
- `GET /api/v1/login` → demo login handler
- JSON body parsing (`express.json()`)
- Global error handler middleware

## Folder structure

```
server/
├─ src/
│  ├─ app.ts                      # Express app (middleware + routes)
│  ├─ server.ts                   # Bootstraps app.listen
│  ├─ controllers/
│  │  └─ authController.ts
│  ├─ config/
│  │  └─ config.ts                # Loads env via dotenv + exposes config
│  └─ middlewares/
│     └─ errorHandler.ts
│  └─ routes/
│     └─ authRoutes.ts
├─ nodemon.json                   # dev runner (ts-node ./src/server.ts)
├─ tsconfig.json
├─ package.json
├─ .env
├─ eslintrc.js
└─ .prettierrc
```

## Manual setup (from scratch)

This is a copy-paste friendly reference if you ever want to recreate this same setup.

### 1) Create folder + init

```bash
mkdir server && cd server

# initialize package.json (pick one)
npm init -y
# bun init
```

### 2) Install dependencies

Runtime deps:

```bash
bun add express dotenv
```

Dev deps (for TypeScript + dev server):

```bash
bun add -d typescript ts-node nodemon @types/express @types/node
```

Optional (if you want to use the included lint/format configs):

```bash
bun add -d eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-config-prettier
```

### 3) Add scripts

`package.json` (scripts):

```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon",
    "lint": "eslint 'src/**/*.ts'",
    "build": "tsc",
    "start": "node dist/server.js"
  }
}
```

### 4) Config files

`tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "esnext",
    "rootDir": "./src",
    "outDir": "./dist",
    "esModuleInterop": true,
    "module": "nodenext",
    "strict": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

`nodemon.json`

```json
{
  "watch": ["src"],
  "ignore": ["src/**/*.spec.ts"],
  "ext": "ts, html, css, ejs, json",
  "exec": "ts-node ./src/server.ts"
}
```

`eslintrc.js` (optional)

```js
module.exports = {
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier',
  ],
  env: {
    node: true,
    es6: true,
  },
};
```

`.prettierrc` (optional)

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all"
}
```

### 5) App code

`src/config/config.ts`

```ts
import dotenv from 'dotenv';

dotenv.config();

interface Config {
  port: number;
  nodeEnv: string;
}

const config: Config = {
  port: Number(process.env.PORT) || 5000,
  nodeEnv: process.env.NODE_ENV || 'development',
};

export default config;
```

`src/middlewares/errorHandler.ts`

```ts
import { Request, Response, NextFunction } from 'express';

export interface AppError extends Error {
  status?: number;
}

export const errorHandler = (
  err: AppError,
  req: Request,
  res: Response,
  next: NextFunction,
) => {
  console.error(err);
  res.status(err.status || 500).json({
    message: err.message || 'Internal Server Error',
  });
};
```

`src/controllers/authController.ts`

```ts
import type { Request, Response } from 'express';

const signup = (req: Request, res: Response) => {
  res.send('Signup sucessfull');
};

const login = (req: Request, res: Response) => {
  res.send('Login successfull');
};

export { signup, login };
```

`src/routes/authRoutes.ts`

```ts
import express from 'express';
import { login, signup } from '../controllers/authController';

const router = express.Router();

router.get('/signup', signup);
router.get('/login', login);

export default router;
```

`src/app.ts`

```ts
import express from "express";
import { errorHandler } from "../src/middlewares/errorHandler";
import authRoutes from "./routes/authRoutes";

const app = express();
app.use(express.json());

// API routes
app.use("/api/v1", authRoutes);

// Global error handler (should be after routes)
app.use(errorHandler);

app.get('/', (req, res) => {
app.get("/", (req, res) => {
  res.send("server running just fine");
});

export default app;
```

`src/server.ts`

```ts
import app from './app';
import config from './config/config';

app.listen(config.port, () => {
  console.log(`server is running on port`, config.port);
});
```

Development (nodemon + ts-node):

```bash
npm run dev
```

Production build + start:

```bash
npm run build
npm start
```

## Environment

Create `server/.env` if needed:

```env
PORT=5000
NODE_ENV=development
```

## What’s implemented

- `GET /` → returns a simple “server running just fine” response
- JSON body parsing (`express.json()`)
- Global error handler middleware

## Folder structure

```
server/
├─ src/
│  ├─ app.ts                      # Express app (middleware + routes)
│  ├─ server.ts                   # Bootstraps app.listen
│  ├─ config/
│  │  └─ config.ts                # Loads env via dotenv + exposes config
│  └─ middlewares/
│     └─ errorHandler.ts
├─ nodemon.json                   # dev runner (ts-node ./src/server.ts)
├─ tsconfig.json
├─ package.json
├─ .env
├─ eslintrc.js
└─ .prettierrc
```

## Manual setup (from scratch)

This is a copy-paste friendly reference if you ever want to recreate this same setup.

### 1) Create folder + init

```bash
mkdir server && cd server

# initialize package.json (pick one)
npm init -y
# bun init
```

### 2) Install dependencies

Runtime deps:

```bash
bun add express dotenv
```

Dev deps (for TypeScript + dev server):

```bash
bun add -d typescript ts-node nodemon @types/express @types/node
```

Optional (if you want to use the included lint/format configs):

```bash
bun add -d eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-config-prettier
```

### 3) Add scripts

`package.json` (scripts):

```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "nodemon",
    "lint": "eslint 'src/**/*.ts'",
    "build": "tsc",
    "start": "node dist/server.js"
  }
}
```

### 4) Config files

`tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "esnext",
    "rootDir": "./src",
    "outDir": "./dist",
    "esModuleInterop": true,
    "module": "nodenext",
    "strict": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*.ts"],
  "exclude": ["node_modules"]
}
```

`nodemon.json`

```json
{
  "watch": ["src"],
  "ignore": ["src/**/*.spec.ts"],
  "ext": "ts, html, css, ejs, json",
  "exec": "ts-node ./src/server.ts"
}
```

`eslintrc.js` (optional)

```js
module.exports = {
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended',
    'prettier',
  ],
  env: {
    node: true,
    es6: true,
  },
};
```

`.prettierrc` (optional)

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all"
}
```

### 5) App code

`src/config/config.ts`

```ts
import dotenv from 'dotenv';

dotenv.config();

interface Config {
  port: number;
  nodeEnv: string;
}

const config: Config = {
  port: Number(process.env.PORT) || 5000,
  nodeEnv: process.env.NODE_ENV || 'development',
};

export default config;
```

`src/middlewares/errorHandler.ts`

```ts
import { Request, Response, NextFunction } from 'express';

export interface AppError extends Error {
  status?: number;
}

export const errorHandler = (
  err: AppError,
  req: Request,
  res: Response,
  next: NextFunction,
) => {
  console.error(err);
  res.status(err.status || 500).json({
    message: err.message || 'Internal Server Error',
  });
};
```

`src/app.ts`

```ts
import express from 'express';
import { errorHandler } from '../src/middlewares/errorHandler';

const app = express();
app.use(express.json());

// Global error handler (should be after routes)
app.use(errorHandler);

app.get('/', (req, res) => {
  res.send('server running just fine');
});

export default app;
```

`src/server.ts`

```ts
import app from './app';
import config from './config/config';

app.listen(config.port, () => {
  console.log(`server is running on port`, config.port);
});
```
