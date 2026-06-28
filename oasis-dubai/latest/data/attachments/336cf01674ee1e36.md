# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: ../setup/auth.setup.ts >> authenticate as admin
- Location: tests/e2e/setup/auth.setup.ts:6:6

# Error details

```
Error: No token in login response

expect(received).toBeTruthy()

Received: undefined
```

# Test source

```ts
  1  | import { test as setup, expect } from '@playwright/test';
  2  | import path from 'path';
  3  | 
  4  | const AUTH_FILE = path.resolve(__dirname, '../.auth/admin.json');
  5  | 
  6  | setup('authenticate as admin', async ({ page, request }) => {
  7  |   const apiUrl = process.env.E2E_API_URL;
  8  |   const email = process.env.E2E_ADMIN_EMAIL;
  9  |   const password = process.env.E2E_ADMIN_PASSWORD;
  10 | 
  11 |   if (!apiUrl || !email || !password) {
  12 |     throw new Error(
  13 |       'Missing E2E env vars. Copy tests/e2e/.env.e2e.example to .env.e2e and fill in credentials.'
  14 |     );
  15 |   }
  16 | 
  17 |   // Step 1: Login via API (fast, no browser overhead)
  18 |   const loginResponse = await request.post(`${apiUrl}/login`, {
  19 |     data: { email, password },
  20 |   });
  21 |   expect(loginResponse.ok(), `Login failed: ${loginResponse.status()}`).toBeTruthy();
  22 | 
  23 |   const { token } = await loginResponse.json();
> 24 |   expect(token, 'No token in login response').toBeTruthy();
     |                                               ^ Error: No token in login response
  25 | 
  26 |   // Step 2: Inject token into localStorage via browser
  27 |   await page.goto('/login', { waitUntil: 'commit' });
  28 |   await page.evaluate((t: string) => {
  29 |     localStorage.setItem('AUTH_TOKEN', t);
  30 |   }, token);
  31 | 
  32 |   // Step 3: Save storageState (localStorage + cookies) for all workers.
  33 |   await page.context().storageState({ path: AUTH_FILE });
  34 | });
  35 | 
```