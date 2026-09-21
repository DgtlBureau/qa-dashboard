# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 08-match-page.spec.js >> Страница матча из календаря открывается @major
- Location: tests/e2e/08-match-page.spec.js:6:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/season/calendar/
Call log:
  - navigating to "https://dev.hctorpedo.ru/season/calendar/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: страница матча из календаря открывается.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | test('Страница матча из календаря открывается @major', async ({ page }) => {
> 7  |   await page.goto('/season/calendar/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/season/calendar/
  8  |   await page.waitForLoadState('networkidle', { timeout: 15000 }).catch(() => {});
  9  | 
  10 |   // Находим первую ссылку на матч
  11 |   const matchLinks = await page.locator('a[href*="/season/"], a[href*="/online/"]').evaluateAll(
  12 |     (els) => els.map((a) => a.getAttribute('href')).filter(Boolean).slice(0, 5)
  13 |   );
  14 | 
  15 |   if (matchLinks.length === 0) {
  16 |     test.skip();
  17 |     return;
  18 |   }
  19 | 
  20 |   const href = matchLinks[0];
  21 |   const resp = await page.goto(href, { waitUntil: 'domcontentloaded', timeout: 15000 });
  22 |   expect(resp.status(), `Страница матча ${href} вернула ${resp.status()}`).not.toBe(404);
  23 | 
  24 |   const bodyText = await page.textContent('body');
  25 |   expect(bodyText.replace(/\s+/g, ' ').trim().length, 'Страница матча пустая').toBeGreaterThan(50);
  26 | });
  27 | 
```