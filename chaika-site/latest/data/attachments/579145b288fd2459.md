# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 18-tickets.spec.js >> Кнопка «КУПИТЬ АБОНЕМЕНТ» работает @critical
- Location: tests/e2e/18-tickets.spec.js:6:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-chaika.hctorpedo.ru/tickets/season/2026-2027/
Call log:
  - navigating to "https://dev-chaika.hctorpedo.ru/tickets/season/2026-2027/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Билеты: кнопка «КУПИТЬ АБОНЕМЕНТ», доступность страниц.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | test('Кнопка «КУПИТЬ АБОНЕМЕНТ» работает @critical', async ({ page }) => {
  7  |   const now = new Date();
  8  |   const seasonStart = now.getMonth() >= 8 ? now.getFullYear() : now.getFullYear() - 1;
  9  |   const seasonPath = `/tickets/season/${seasonStart}-${seasonStart + 1}/`;
  10 | 
> 11 |   await page.goto(seasonPath, { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-chaika.hctorpedo.ru/tickets/season/2026-2027/
  12 | 
  13 |   const btn = page.locator('button:has-text("КУПИТЬ АБОНЕМЕНТ"), a:has-text("КУПИТЬ АБОНЕМЕНТ")').first();
  14 |   await btn.waitFor({ timeout: 15000 });
  15 | 
  16 |   await btn.click();
  17 | 
  18 |   // Ждём реакции: модалка или переход
  19 |   await page.waitForLoadState('networkidle', { timeout: 5000 }).catch(() => {});
  20 | 
  21 |   const modalVisible = await page.locator('[class*="modal"]:visible, [role="dialog"]:visible, [class*="popup"]:visible').count();
  22 |   const urlChanged = !page.url().includes(seasonPath);
  23 | 
  24 |   expect(
  25 |     modalVisible > 0 || urlChanged,
  26 |     'Клик по «КУПИТЬ АБОНЕМЕНТ» не открыл модалку и не сделал переход — кнопка не работает'
  27 |   ).toBeTruthy();
  28 | });
  29 | 
  30 | test('/tickets/season/ — страница доступна (не 403) @major', async ({ page }) => {
  31 |   const resp = await page.goto('/tickets/season/', { waitUntil: 'domcontentloaded' });
  32 |   expect(
  33 |     resp.status(),
  34 |     `/tickets/season/ вернул ${resp.status()} (ожидается 200)`
  35 |   ).toBe(200);
  36 | });
  37 | 
```