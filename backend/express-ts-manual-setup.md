# Express TypeScript Backend Setup Guide

The goal is to demonstrate best practices for building a robust, type-safe Express application using TypeScript.

## 1. Initialize the Project

```bash
mkdir ts-node-express && cd ts-node-express
npm init -y
```

**Install dependencies:**

```bash
npm install express dotenv
npm install -D typescript ts-node @types/node @types/express nodemon eslint prettier
```

## 2. Configure TypeScript

```bash
npx tsc --init
```

**Update `tsconfig.json`:**

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

## 3. Project Structure

```
ts-node-express/
├── src/
│   ├── config/
│   │   └── config.ts           # Load and type environment variables
│   ├── controllers/
│   │   └── itemController.ts   # CRUD logic for "items"
│   ├── middlewares/
│   │   └── errorHandler.ts     # Global typed error handling middleware
│   ├── models/
│   │   └── item.ts             # Define item type and in-memory storage
│   ├── routes/
│   │   └── itemRoutes.ts       # Express routes for items
│   ├── app.ts                  # Express app configuration (middlewares, routes)
│   └── server.ts               # Start the server
├── .env                        # Environment variables
├── package.json                # Project scripts, dependencies, etc.
├── tsconfig.json               # TypeScript configuration
├── .eslintrc.js                # ESLint configuration
└── .prettierrc                 # Prettier configuration
```

## 4. Environment Configuration

**`src/config/config.ts`** - Typed environment variables:

```typescript
import dotenv from 'dotenv';

dotenv.config();

interface Config {
  port: number;
  nodeEnv: string;
}

const config: Config = {
  port: Number(process.env.PORT) || 3000,
  nodeEnv: process.env.NODE_ENV || 'development',
};

export default config;
```

## 5. Environment Variables

**`.env`:**

```env
PORT=3000
NODE_ENV=development
```

## 6. Global Error Handling

**`src/middlewares/errorHandler.ts`:**

```typescript
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

## 7. Routes

**`src/routes/itemRoutes.ts`:**

```typescript
import { Router } from 'express';
import {
  createItem,
  getItems,
  getItemById,
  updateItem,
  deleteItem,
} from '../controllers/itemController';

const router = Router();

router.get('/', getItems);
router.get('/:id', getItemById);
router.post('/', createItem);
router.put('/:id', updateItem);
router.delete('/:id', deleteItem);

export default router;
```

## 8. Models Setup

**`src/models/item.ts`** - In-memory data:

```typescript
export interface Item {
  id: number;
  name: string;
}

export let items: Item[] = [];
```

## 9. Controller (CRUD Logic)

**`src/controllers/itemController.ts`:**

```typescript
import { Request, Response, NextFunction } from 'express';
import { items, Item } from '../models/item';

// Create an item
export const createItem = (req: Request, res: Response, next: NextFunction) => {
  try {
    const { name } = req.body;
    const newItem: Item = { id: Date.now(), name };
    items.push(newItem);
    res.status(201).json(newItem);
  } catch (error) {
    next(error);
  }
};

// Read all items
export const getItems = (req: Request, res: Response, next: NextFunction) => {
  try {
    res.json(items);
  } catch (error) {
    next(error);
  }
};

// Read single item
export const getItemById = (
  req: Request,
  res: Response,
  next: NextFunction,
) => {
  try {
    const id = parseInt(req.params.id, 10);
    const item = items.find((i) => i.id === id);
    if (!item) {
      res.status(404).json({ message: 'Item not found' });
      return;
    }
    res.json(item);
  } catch (error) {
    next(error);
  }
};

// Update an item
export const updateItem = (req: Request, res: Response, next: NextFunction) => {
  try {
    const id = parseInt(req.params.id, 10);
    const { name } = req.body;
    const itemIndex = items.findIndex((i) => i.id === id);
    if (itemIndex === -1) {
      res.status(404).json({ message: 'Item not found' });
      return;
    }
    items[itemIndex].name = name;
    res.json(items[itemIndex]);
  } catch (error) {
    next(error);
  }
};

// Delete an item
export const deleteItem = (req: Request, res: Response, next: NextFunction) => {
  try {
    const id = parseInt(req.params.id, 10);
    const itemIndex = items.findIndex((i) => i.id === id);
    if (itemIndex === -1) {
      res.status(404).json({ message: 'Item not found' });
      return;
    }
    const deletedItem = items.splice(itemIndex, 1)[0];
    res.json(deletedItem);
  } catch (error) {
    next(error);
  }
};
```

## 10. App Setup

**`src/app.ts`:**

```typescript
import express from 'express';
import itemRoutes from './routes/itemRoutes';
import { errorHandler } from './middlewares/errorHandler';

const app = express();

app.use(express.json());

// Routes
app.use('/api/items', itemRoutes);

// Global error handler (should be after routes)
app.use(errorHandler);

export default app;
```

## 11. Server Entry Point

**`src/server.ts`:**

```typescript
import app from './app';
import config from './config/config';

app.listen(config.port, () => {
  console.log(`Server running on port ${config.port}`);
});
```

## 12. Linting and Code Formatting

**`.eslintrc.js`:**

```javascript
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

**`.prettierrc`:**

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all"
}
```

## 13. Development Scripts

**`package.json`:**

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/server.js",
    "dev": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/server.ts",
    "lint": "eslint 'src/**/*.ts'",
    "test": "jest"
  }
}
```

## 14. Getting Started

```bash
npm run dev
# or
bun run dev
```

## 15. Testing Setup (Optional)

**Install Jest and ts-jest:**

```bash
npm install --save-dev jest ts-jest @types/jest
```

**`jest.config.js`:**

```javascript
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node',
  moduleFileExtensions: ['ts', 'js'],
  testMatch: ['**/tests/**/*.test.(ts|js)'],
  globals: {
    'ts-jest': {
      tsconfig: 'tsconfig.json',
    },
  },
};
```

**`tests/itemController.test.ts`:**

```typescript
import { Request, Response } from 'express';
import { getItems } from '../src/controllers/itemController';
import { items } from '../src/models/item';

describe('Item Controller', () => {
  it('should return an empty array when no items exist', () => {
    const req = {} as Request;
    const res = {
      json: jest.fn(),
    } as unknown as Response;

    items.length = 0;

    getItems(req, res, jest.fn());

    expect(res.json).toHaveBeenCalledWith([]);
  });
});
```

---

**You're all set!** Start coding with `npm run dev` and test your API endpoints.
