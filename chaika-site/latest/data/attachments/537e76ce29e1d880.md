# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 09-console-errors.spec.js >> Календарь /season/calendar/ — нет JS-ошибок в консоли @major
- Location: tests/e2e/09-console-errors.spec.js:23:7

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-chaika.hctorpedo.ru/season/calendar/
Call log:
  - navigating to "https://dev-chaika.hctorpedo.ru/season/calendar/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: JS-ошибки в консоли на ключевых страницах.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | const PAGES = [
  7  |   { path: '/', name: 'Главная' },
  8  |   { path: '/news/', name: 'Новости' },
  9  |   { path: '/teamroster/', name: 'Состав' },
  10 |   { path: '/season/calendar/', name: 'Календарь' },
  11 |   { path: '/season/tournament_tables/', name: 'Турнирная таблица' },
  12 |   { path: '/tickets/', name: 'Билеты' },
  13 |   { path: '/video/', name: 'Видео' },
  14 | ];
  15 | 
  16 | const IGNORED_PATTERNS = [
  17 |   'favicon', 'Failed to load resource', 'third-party',
  18 |   'google', 'yandex', 'metrika', 'mc.yandex',
  19 |   'googletagmanager', 'analytics', 'crm.hctorpedo',
  20 | ];
  21 | 
  22 | for (const pg of PAGES) {
  23 |   test(`${pg.name} ${pg.path} — нет JS-ошибок в консоли @major`, async ({ page }) => {
  24 |     const errors = [];
  25 | 
  26 |     page.on('pageerror', (error) => {
  27 |       errors.push(error.message);
  28 |     });
  29 | 
  30 |     page.on('console', (msg) => {
  31 |       if (msg.type() === 'error') {
  32 |         const text = msg.text();
  33 |         if (IGNORED_PATTERNS.some((p) => text.includes(p))) return;
  34 |         errors.push(`console.error: ${text}`);
  35 |       }
  36 |     });
  37 | 
> 38 |     await page.goto(pg.path, { waitUntil: 'domcontentloaded' });
     |                ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-chaika.hctorpedo.ru/season/calendar/
  39 |     await page.waitForLoadState('networkidle', { timeout: 10000 }).catch(() => {});
  40 | 
  41 |     expect(
  42 |       errors,
  43 |       `JS-ошибки на ${pg.path}:\n${errors.join('\n')}`
  44 |     ).toHaveLength(0);
  45 |   });
  46 | }
  47 | 
```