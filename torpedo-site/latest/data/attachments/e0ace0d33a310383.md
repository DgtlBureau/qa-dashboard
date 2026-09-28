# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 16-photo-freshness.spec.js >> Фотогалерея: последний альбом не старше 30 дней @major
- Location: tests/e2e/16-photo-freshness.spec.js:7:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/photogallery/
Call log:
  - navigating to "https://dev.hctorpedo.ru/photogallery/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Фотогалерея: актуальность альбомов, обои.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | import { parseRuDate } from './config.js';
  6  | 
  7  | test('Фотогалерея: последний альбом не старше 30 дней @major', async ({ page }) => {
> 8  |   await page.goto('/photogallery/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/photogallery/
  9  |   await page.locator('.list-news__item-date, [class*="date"]').first().waitFor({ timeout: 15000 });
  10 | 
  11 |   const dateText = await page.locator('.list-news__item-date, [class*="date"]')
  12 |     .first().textContent().catch(() => null);
  13 |   expect(dateText, 'Не удалось найти дату альбома').toBeTruthy();
  14 | 
  15 |   const albumDate = parseRuDate(dateText);
  16 |   expect(albumDate, `Не удалось распарсить дату: "${dateText}"`).not.toBeNull();
  17 | 
  18 |   const daysDiff = (Date.now() - albumDate.getTime()) / (1000 * 60 * 60 * 24);
  19 |   expect(
  20 |     daysDiff,
  21 |     `Последний альбом от ${dateText.trim()} — ${Math.floor(daysDiff)} дней назад`
  22 |   ).toBeLessThanOrEqual(30);
  23 | });
  24 | 
  25 | test('Обои /photogallery/wallpaper/ — страница доступна (не 403) @major', async ({ page }) => {
  26 |   const resp = await page.goto('/photogallery/wallpaper/', { waitUntil: 'domcontentloaded' });
  27 |   expect(
  28 |     resp.status(),
  29 |     `/photogallery/wallpaper/ вернул ${resp.status()} (ожидается 200)`
  30 |   ).toBe(200);
  31 | });
  32 | 
```