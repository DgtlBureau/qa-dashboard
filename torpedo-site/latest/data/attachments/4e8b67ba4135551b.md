# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 01-availability.spec.js >> Главная / отвечает 200 @critical
- Location: tests/e2e/01-availability.spec.js:8:7

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/
Call log:
  - navigating to "https://dev.hctorpedo.ru/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: доступность ключевых страниц.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | import { COMMON_PAGES } from './config.js';
  6  | 
  7  | for (const pg of COMMON_PAGES) {
  8  |   test(`${pg.name} ${pg.path} отвечает 200 @critical`, async ({ page }) => {
> 9  |     const resp = await page.goto(pg.path, { waitUntil: 'domcontentloaded' });
     |                             ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/
  10 |     expect(resp.status(), `${pg.path} вернул ${resp.status()}`).toBe(200);
  11 |   });
  12 | }
  13 | 
```