# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 17-broadcasts.spec.js >> /online/broadcasts/2026-03/ — HTTP 200 @minor
- Location: tests/e2e/17-broadcasts.spec.js:23:7

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/online/broadcasts/2026-03/
Call log:
  - navigating to "https://dev.hctorpedo.ru/online/broadcasts/2026-03/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Трансляции: помесячные архивы доступны.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | // Генерируем месяцы текущего хоккейного сезона (сентябрь — март)
  7  | function getSeasonMonths() {
  8  |   const now = new Date();
  9  |   const year = now.getMonth() >= 8 ? now.getFullYear() : now.getFullYear() - 1;
  10 |   const months = [];
  11 |   for (let m = 9; m <= 12; m++) {
  12 |     months.push(`/online/broadcasts/${year}-${String(m).padStart(2, '0')}/`);
  13 |   }
  14 |   for (let m = 1; m <= 3; m++) {
  15 |     months.push(`/online/broadcasts/${year + 1}-${String(m).padStart(2, '0')}/`);
  16 |   }
  17 |   return months;
  18 | }
  19 | 
  20 | const BROADCAST_MONTHS = getSeasonMonths();
  21 | 
  22 | for (const path of BROADCAST_MONTHS) {
  23 |   test(`${path} — HTTP 200 @minor`, async ({ page }) => {
> 24 |     const resp = await page.goto(path, { waitUntil: 'domcontentloaded', timeout: 15000 });
     |                             ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/online/broadcasts/2026-03/
  25 |     expect(
  26 |       resp.status(),
  27 |       `${path} вернул ${resp.status()}`
  28 |     ).toBe(200);
  29 |   });
  30 | }
  31 | 
```