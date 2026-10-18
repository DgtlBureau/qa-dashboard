# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 15-video-categories.spec.js >> Видео: категория «Промо» не пустая @major
- Location: tests/e2e/15-video-categories.spec.js:15:7

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/video/
Call log:
  - navigating to "https://dev-gorky.hctorpedo.ru/video/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Видео: проверка категорий, актуальности и количества.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | import { parseRuDate } from './config.js';
  6  | 
  7  | const CATEGORIES = [
  8  |   'Хайлайты', 'События', 'Поехали', 'Интервью', 'Промо',
  9  |   'LIVE', 'Игроки', 'Чарты', 'Творчество', 'Документальное',
  10 | ];
  11 | 
  12 | // ─── Категории ───────────────────────────────────────────────────────────────
  13 | 
  14 | for (const cat of CATEGORIES) {
  15 |   test(`Видео: категория «${cat}» не пустая @major`, async ({ page }) => {
> 16 |     await page.goto('/video/', { waitUntil: 'domcontentloaded' });
     |                ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/video/
  17 |     await page.locator('.list-news__item').first().waitFor({ timeout: 15000 });
  18 | 
  19 |     // Кликаем на кнопку категории
  20 |     const btn = page.locator(`button.dropdown__toggler:has-text("${cat}")`);
  21 |     expect(await btn.count(), `Кнопка категории «${cat}» не найдена`).toBeGreaterThan(0);
  22 |     await btn.click();
  23 |     await page.waitForResponse(resp => resp.url().includes('/video') && resp.status() === 200, { timeout: 10000 }).catch(() => {});
  24 | 
  25 |     // Проверяем что появились видео
  26 |     const items = await page.locator('.list-news__item').count();
  27 |     expect(
  28 |       items,
  29 |       `Категория «${cat}» пуста — 0 видео после клика`
  30 |     ).toBeGreaterThan(0);
  31 |   });
  32 | }
  33 | 
  34 | // ─── Актуальность ────────────────────────────────────────────────────────────
  35 | 
  36 | test('Видео: последнее не старше 30 дней @major', async ({ page }) => {
  37 |   await page.goto('/video/', { waitUntil: 'domcontentloaded' });
  38 |   await page.locator('.list-news__item-date').first().waitFor({ timeout: 15000 });
  39 | 
  40 |   const dateText = await page.locator('.list-news__item-date').first().textContent().catch(() => null);
  41 |   expect(dateText, 'Не удалось найти дату видео').toBeTruthy();
  42 | 
  43 |   const videoDate = parseRuDate(dateText);
  44 |   expect(videoDate, `Не удалось распарсить дату: "${dateText}"`).not.toBeNull();
  45 | 
  46 |   const daysDiff = (Date.now() - videoDate.getTime()) / (1000 * 60 * 60 * 24);
  47 |   expect(
  48 |     daysDiff,
  49 |     `Последнее видео от ${dateText.trim()} — ${Math.floor(daysDiff)} дней назад`
  50 |   ).toBeLessThanOrEqual(30);
  51 | });
  52 | 
  53 | // ─── Сравнение с prod ────────────────────────────────────────────────────────
  54 | 
  55 | test('Видео: количество на dev ≈ prod (отклонение ≤10%) @major', async ({ browser }) => {
  56 |   const ctx = await browser.newContext();
  57 | 
  58 |   const devPage = await ctx.newPage();
  59 |   await devPage.goto('/video/', { waitUntil: 'domcontentloaded' });
  60 |   await devPage.locator('text=/из/').first().waitFor({ timeout: 15000 }).catch(() => {});
  61 |   const devText = await devPage.locator('text=/из/').first().textContent().catch(() => '');
  62 |   const devMatch = devText.match(/из\s+([\d\s]+)/);
  63 |   const devTotal = devMatch ? parseInt(devMatch[1].replace(/\s/g, '')) : 0;
  64 | 
  65 |   const prodPage = await ctx.newPage();
  66 |   await prodPage.goto('https://hctorpedo.ru/video/', { waitUntil: 'domcontentloaded' });
  67 |   await prodPage.locator('text=/из/').first().waitFor({ timeout: 15000 }).catch(() => {});
  68 |   const prodText = await prodPage.locator('text=/из/').first().textContent().catch(() => '');
  69 |   const prodMatch = prodText.match(/из\s+([\d\s]+)/);
  70 |   const prodTotal = prodMatch ? parseInt(prodMatch[1].replace(/\s/g, '')) : 0;
  71 | 
  72 |   await ctx.close();
  73 | 
  74 |   expect(devTotal, 'Не удалось получить количество видео на dev').toBeGreaterThan(0);
  75 |   expect(prodTotal, 'Не удалось получить количество видео на prod').toBeGreaterThan(0);
  76 | 
  77 |   const diffPercent = Math.abs(devTotal - prodTotal) / prodTotal * 100;
  78 |   expect(
  79 |     diffPercent,
  80 |     `Dev: ${devTotal}, Prod: ${prodTotal} — разница ${Math.round(diffPercent)}% (${Math.abs(devTotal - prodTotal)} видео)`
  81 |   ).toBeLessThanOrEqual(10);
  82 | });
  83 | 
```