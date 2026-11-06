# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 05-links.spec.js >> Ссылки в футере не возвращают 404 @minor
- Location: tests/e2e/05-links.spec.js:6:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/
Call log:
  - navigating to "https://dev-gorky.hctorpedo.ru/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: ссылки в футере не битые.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | test('Ссылки в футере не возвращают 404 @minor', async ({ page }) => {
> 7  |   await page.goto('/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/
  8  | 
  9  |   const footerLinks = await page.locator('footer a[href]').evaluateAll((els) =>
  10 |     els
  11 |       .map((a) => a.getAttribute('href'))
  12 |       .filter((href) => href && !href.startsWith('#') && !href.startsWith('mailto:') && !href.startsWith('tel:'))
  13 |       .filter((href) => href.startsWith('/') || href.includes('hctorpedo'))
  14 |   );
  15 | 
  16 |   const uniqueLinks = [...new Set(footerLinks)].slice(0, 20);
  17 |   const broken = [];
  18 | 
  19 |   for (const href of uniqueLinks) {
  20 |     try {
  21 |       const resp = await page.goto(href, { waitUntil: 'domcontentloaded', timeout: 15000 });
  22 |       if (resp.status() === 404) {
  23 |         broken.push({ href, status: resp.status() });
  24 |       }
  25 |     } catch {
  26 |       broken.push({ href, status: 'timeout/error' });
  27 |     }
  28 |   }
  29 | 
  30 |   expect(
  31 |     broken,
  32 |     `Битые ссылки в футере:\n${broken.map((b) => `  ${b.href} → ${b.status}`).join('\n')}`
  33 |   ).toHaveLength(0);
  34 | });
  35 | 
```