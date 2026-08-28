# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 06-fonts.spec.js >> /video/ — заголовки используют Rodchenko @minor
- Location: tests/e2e/06-fonts.spec.js:14:7

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/video/
Call log:
  - navigating to "https://dev.hctorpedo.ru/video/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: шрифты Rodchenko и Play на ключевых страницах.
  3  |  * Допустимые шрифты: Rodchenko (заголовки), Play (текст).
  4  |  * Roboto и другие — баг.
  5  |  */
  6  | import { test, expect } from '@playwright/test';
  7  | 
  8  | const PAGES = [
  9  |   '/', '/news/', '/teamroster/', '/season/calendar/',
  10 |   '/tickets/', '/video/', '/contacts/',
  11 | ];
  12 | 
  13 | for (const path of PAGES) {
  14 |   test(`${path} — заголовки используют Rodchenko @minor`, async ({ page }) => {
> 15 |     await page.goto(path, { waitUntil: 'domcontentloaded' });
     |                ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/video/
  16 | 
  17 |     const heading = page.locator('h1, h2, h3').first();
  18 |     if ((await heading.count()) === 0) {
  19 |       test.skip();
  20 |       return;
  21 |     }
  22 | 
  23 |     const fontFamily = await heading.evaluate(
  24 |       (el) => getComputedStyle(el).fontFamily
  25 |     );
  26 |     expect(
  27 |       fontFamily.toLowerCase(),
  28 |       `${path}: заголовок использует "${fontFamily}" вместо Rodchenko`
  29 |     ).toContain('rodchenko');
  30 |   });
  31 | 
  32 |   test(`${path} — текст использует Play @minor`, async ({ page }) => {
  33 |     await page.goto(path, { waitUntil: 'domcontentloaded' });
  34 | 
  35 |     const bodyFont = await page.locator('body').evaluate(
  36 |       (el) => getComputedStyle(el).fontFamily
  37 |     );
  38 |     expect(
  39 |       bodyFont.toLowerCase(),
  40 |       `${path}: текст использует "${bodyFont}" вместо Play`
  41 |     ).toContain('play');
  42 |   });
  43 | }
  44 | 
```