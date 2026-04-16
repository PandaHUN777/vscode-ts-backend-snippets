# VS Code TypeScript Backend Snippets

A personal, growing collection of VS Code snippets for building Node.js / Express backends in TypeScript. Copy `typescript.json` into your VS Code snippets folder and get instant boilerplate for servers, routes, controllers, services, middleware, and Jest tests.

> Focused on backend development. More snippets are added as real patterns emerge from building apps.

---

## Installation

1. Open VS Code.
2. Press `Cmd+Shift+P` (macOS) / `Ctrl+Shift+P` (Windows/Linux).
3. Run **"Snippets: Configure User Snippets"** → choose **"typescript"**.
4. Replace the contents of the file with [`typescript.json`](./typescript.json) from this repo (or merge the entries if you already have snippets).

That's it. The snippets are available in any `.ts` or `.tsx` file.

---

## Snippets

| Prefix | Name | Description |
|---|---|---|
| `server` | Express Server | Express app entry point with CORS and JSON body parsing |
| `route` | Route File | Express Router with GET and POST handlers wired to a controller |
| `controller` | Controller | Async controller function with try/catch and service call |
| `service` | Service Class | Typed service class with an async method skeleton |
| `types` | Types File | Status union type + entity, request, and response interfaces |
| `mockdb` | Map Mock DB | In-memory Map as a mock database (no external dependency) |
| `idempotency` | Idempotency Check | Guard block to skip re-processing a duplicate request |
| `middleware` | Middleware | Generic Express middleware with error forwarding |
| `err-middleware` | Error Middleware | 4-argument error-handling middleware (Express convention) |
| `authmiddleware` | Auth Middleware | Bearer token extraction and auth guard middleware |
| `test` | Jest Test | Jest `describe` + `beforeEach` + `it` block for a service |

---

## Snippet Details

### `server` — Express Server

Scaffolds a complete `index.ts` / `server.ts` entry point.

```ts
import express from 'express';
import cors from 'cors';

const app = express();
const PORT = process.env.PORT ?? 3000;

app.use(cors());
app.use(express.json());

// Routes

app.get('/health', (req, res) => {
  res.json({ status: 'ok' });
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

---

### `route` — Route File

Scaffolds an Express router. Tab stops: `controllerName`, `controllerFile`, `path`.

```ts
import { Router } from 'express';
import { controllerName } from '../controllers/controllerFile.ts';

const router = Router();

router.get('/path', controllerName);
router.post('/path', controllerName);

export default router;
```

---

### `controller` — Controller

Async controller that pulls from `req.body`, calls a service method, and returns JSON. Tab stops: `ServiceName`, `serviceName`, `functionName`, destructured body fields, `methodName`.

```ts
import type { Request, Response } from 'express';
import { ServiceName } from '../services/serviceName.ts';

const service = new ServiceName();

export const functionName = async (
  req: Request,
  res: Response
): Promise<void> => {
  try {
    const { } = req.body;

    const result = await service.methodName();

    res.status(200).json({ data: result });
  } catch (error) {
    console.error('Error:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
};
```

---

### `service` — Service Class

Typed service class with one async method. Tab stops: `TypeName`, `typesFile`, `ServiceName`, `methodName`, `param`, `ParamType`, `ReturnType`.

```ts
import type { TypeName } from '../types/typesFile.ts';

export class ServiceName {

  async methodName(param: ParamType): Promise<ReturnType> {
    try {
      // cursor here
    } catch (error) {
      throw new Error(`methodName failed: ${error}`);
    }
  }

}
```

---

### `types` — Types File

Status union + three interfaces (entity, request, response). Tab stops: `StatusType`, `MainEntity`, `RequestType`, `ResponseType`.

```ts
export type StatusType = 'pending' | 'approved' | 'declined';

export interface MainEntity {
  id: string;
}

export interface RequestType {
}

export interface ResponseType {
}
```

---

### `mockdb` — Map Mock DB

Map-based in-memory store — no database setup needed for prototyping or tests. Tab stops: `entityName`, `EntityType`, `id-001`, `ResponseType`.

```ts
const entityNames = new Map<string, EntityType>([
  ['id-001', {
    id: 'id-001',
  }]
]);

const processedRequests = new Map<string, ResponseType>();
```

---

### `idempotency` — Idempotency Check

Drop this at the top of a controller to short-circuit duplicate requests. Tab stop: `idempotencyKey`.

```ts
// Idempotency check — prevent double processing
const cached = service.isAlreadyProcessed(idempotencyKey);
if (cached) {
  res.status(200).json({ message: 'Already processed', data: cached });
  return;
}
```

---

### `middleware` — Middleware

Generic middleware skeleton with forwarding to `next` on error. Tab stop: `middlewareName`.

```ts
import type { Request, Response, NextFunction } from 'express';

export const middlewareName = (
  req: Request,
  res: Response,
  next: NextFunction
): void => {
  try {
    // logic here
    next();
  } catch (error) {
    next(error);
  }
};
```

---

### `err-middleware` — Error Middleware

4-argument error handler (Express requires all four parameters for the error handler to be recognised).

```ts
import type { Request, Response, NextFunction } from 'express';

export const errorMiddleware = (
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
): void => {
  console.error('Unhandled error:', error);

  const statusCode = (error as any).statusCode ?? 500;
  const message = error.message ?? 'Internal server error';

  res.status(statusCode).json({ error: message });
};
```

---

### `authmiddleware` — Auth Middleware

Extracts and validates a `Bearer` token from the `Authorization` header. Tab stops: `authMiddleware`, then cursor for token verification logic.

```ts
import type { Request, Response, NextFunction } from 'express';

export const authMiddleware = (
  req: Request,
  res: Response,
  next: NextFunction
): void => {
  try {
    const authHeader = req.headers.authorization;

    if (!authHeader?.startsWith('Bearer ')) {
      res.status(401).json({ error: 'Unauthorized' });
      return;
    }

    const token = authHeader.split(' ')[1];
    // verify token here

    next();
  } catch (error) {
    res.status(401).json({ error: 'Invalid token' });
  }
};
```

---

### `test` — Jest Test

Jest test file for a service class. Tab stops: `ServiceName`, `serviceName`, test description. Follows Arrange / Act / Assert comments.

```ts
import { ServiceName } from '../services/serviceName.ts';

describe('ServiceName', () => {
  let service: ServiceName;

  beforeEach(() => {
    service = new ServiceName();
  });

  it('should do something', async () => {
    // Arrange

    // Act

    // Assert
    expect(true).toBe(true);
  });
});
```

---

## Planned Additions

Snippets that will be added as patterns come up in real projects:

- Zod validation schema
- Prisma service method
- JWT sign / verify helpers
- Rate-limit middleware
- Request logger middleware
- Supertest integration test block

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

---

## License

MIT
