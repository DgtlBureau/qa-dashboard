# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 19-match-details.spec.js >> Блок «Предыдущий матч» — страница матча открывается @critical
- Location: tests/e2e/19-match-details.spec.js:47:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/
Call log:
  - navigating to "https://dev.hctorpedo.ru/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Блок «Предыдущий матч»: ссылка «СТРАНИЦА МАТЧА» видима и работает.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | test('Блок «Предыдущий матч» — ссылка «СТРАНИЦА МАТЧА» видима @critical', async ({ page }) => {
  7  |   await page.goto('/', { waitUntil: 'domcontentloaded' });
  8  |   await page.waitForLoadState('networkidle', { timeout: 15000 }).catch(() => {});
  9  | 
  10 |   // Ищем блок предыдущего матча
  11 |   const prevBlock = page.locator('text=/предыдущий матч/i').first();
  12 |   if ((await prevBlock.count()) === 0) {
  13 |     test.skip();
  14 |     return;
  15 |   }
  16 | 
  17 |   // Ищем ВИДИМУЮ ссылку «Страница матча»
  18 |   const matchLink = page.locator('a:has-text("Страница матча"):visible').first();
  19 |   await expect(
  20 |     matchLink,
  21 |     'Ссылка «СТРАНИЦА МАТЧА» скрыта или отсутствует (есть на prod, скрыта на dev)'
  22 |   ).toBeVisible({ timeout: 3000 });
  23 | });
  24 | 
  25 | test('Блок «Предыдущий матч» — ссылка ведёт на свой домен, не на prod @major', async ({ page }) => {
  26 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
  27 |   await page.waitForLoadState('networkidle', { timeout: 15000 }).catch(() => {});
  28 | 
  29 |   const matchLink = page.locator('a:has-text("Страница матча")').first();
  30 |   if ((await matchLink.count()) === 0) {
  31 |     test.skip();
  32 |     return;
  33 |   }
  34 | 
  35 |   const href = await matchLink.getAttribute('href');
  36 |   const baseUrl = test.info().project.use.baseURL || '';
  37 | 
  38 |   // На dev ссылка не должна вести на prod
  39 |   if (baseUrl.includes('dev.hctorpedo')) {
  40 |     expect(
  41 |       href,
  42 |       `Ссылка ведёт на prod: ${href} (должна быть относительная или dev-домен)`
  43 |     ).not.toContain('https://hctorpedo.ru');
  44 |   }
  45 | });
  46 | 
  47 | test('Блок «Предыдущий матч» — страница матча открывается @critical', async ({ page }) => {
> 48 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/
  49 |   await page.waitForLoadState('networkidle', { timeout: 15000 }).catch(() => {});
  50 | 
  51 |   const matchLink = page.locator('a:has-text("Страница матча"):visible').first();
  52 |   if ((await matchLink.count()) === 0) {
  53 |     test.skip();
  54 |     return;
  55 |   }
  56 | 
  57 |   const href = await matchLink.getAttribute('href');
  58 |   // Переходим по ссылке (может быть абсолютной или относительной)
  59 |   const resp = await page.goto(href, { waitUntil: 'domcontentloaded', timeout: 15000 });
  60 |   expect(
  61 |     resp.status(),
  62 |     `Страница матча ${href} вернула ${resp.status()}`
  63 |   ).toBe(200);
  64 | 
  65 |   const body = await page.textContent('body');
  66 |   expect(
  67 |     body.replace(/\s+/g, ' ').trim().length,
  68 |     'Страница матча пустая'
  69 |   ).toBeGreaterThan(50);
  70 | });
  71 | 
```