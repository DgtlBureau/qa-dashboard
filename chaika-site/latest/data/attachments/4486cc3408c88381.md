# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 07-freshness.spec.js >> Последняя новость не старше 7 дней @major
- Location: tests/e2e/07-freshness.spec.js:7:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-chaika.hctorpedo.ru/news/
Call log:
  - navigating to "https://dev-chaika.hctorpedo.ru/news/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: последняя новость не старше 7 дней.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | import { parseRuDate } from './config.js';
  6  | 
  7  | test('Последняя новость не старше 7 дней @major', async ({ page }) => {
> 8  |   await page.goto('/news/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-chaika.hctorpedo.ru/news/
  9  |   await page.locator('.list-news__item, a.list-news__item-wrapper').first().waitFor({ timeout: 15000 });
  10 | 
  11 |   // Ищем дату первой новости
  12 |   const dateSelectors = [
  13 |     '.list-news__item-date',
  14 |     '.list-news__item .list-news__date',
  15 |     '.list-news__item time',
  16 |     '.list-news__item [class*="date"]',
  17 |   ];
  18 | 
  19 |   let dateText = null;
  20 |   for (const sel of dateSelectors) {
  21 |     const el = page.locator(sel).first();
  22 |     if ((await el.count()) > 0) {
  23 |       dateText = await el.textContent().catch(() => null);
  24 |       if (dateText && dateText.trim().length > 3) break;
  25 |       dateText = null;
  26 |     }
  27 |   }
  28 | 
  29 |   // Fallback: ищем паттерн даты в тексте первой новости
  30 |   if (!dateText) {
  31 |     const firstNews = await page.locator('.list-news__item, a.list-news__item-wrapper').first().textContent().catch(() => '');
  32 |     const match = firstNews.match(/\d{1,2}\s+(?:января|февраля|марта|апреля|мая|июня|июля|августа|сентября|октября|ноября|декабря)\s+\d{4}/i)
  33 |       || firstNews.match(/\d{2}\.\d{2}\.\d{4}/);
  34 |     if (match) dateText = match[0];
  35 |   }
  36 | 
  37 |   expect(dateText, 'Не удалось найти дату последней новости на /news/').toBeTruthy();
  38 | 
  39 |   const newsDate = parseRuDate(dateText);
  40 |   expect(newsDate, `Не удалось распарсить дату: "${dateText}"`).not.toBeNull();
  41 | 
  42 |   const daysDiff = (Date.now() - newsDate.getTime()) / (1000 * 60 * 60 * 24);
  43 |   expect(
  44 |     daysDiff,
  45 |     `Последняя новость от ${dateText.trim()} — ${Math.floor(daysDiff)} дней назад`
  46 |   ).toBeLessThanOrEqual(7);
  47 | });
  48 | 
```