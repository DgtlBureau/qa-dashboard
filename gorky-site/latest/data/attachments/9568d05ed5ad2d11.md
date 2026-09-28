# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 02-content.spec.js >> Страница /teamroster/ не содержит серверных ошибок в HTML @major
- Location: tests/e2e/02-content.spec.js:11:7

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/teamroster/
Call log:
  - navigating to "https://dev-gorky.hctorpedo.ru/teamroster/", waiting until "domcontentloaded"

```

# Test source

```ts
  1   | /**
  2   |  * Smoke: контент на месте, нет ошибок.
  3   |  */
  4   | import { test, expect } from '@playwright/test';
  5   | import { PHP_ERROR_PATTERNS, SELECTORS } from './config.js';
  6   | 
  7   | // ─── PHP-ошибки ─────────────────────────────────────────────────────────────
  8   | const PAGES_PHP = ['/', '/news/', '/teamroster/', '/season/calendar/', '/tickets/', '/video/'];
  9   | 
  10  | for (const path of PAGES_PHP) {
  11  |   test(`Страница ${path} не содержит серверных ошибок в HTML @major`, async ({ page }) => {
> 12  |     await page.goto(path, { waitUntil: 'domcontentloaded' });
      |                ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/teamroster/
  13  |     const html = await page.content();
  14  |     for (const pattern of PHP_ERROR_PATTERNS) {
  15  |       expect(html, `PHP-ошибка на ${path}: ${pattern}`).not.toMatch(pattern);
  16  |     }
  17  |   });
  18  | }
  19  | 
  20  | // ─── Контент не пустой ──────────────────────────────────────────────────────
  21  | test('Новости — есть хотя бы одна @major', async ({ page }) => {
  22  |   await page.goto('/news/', { waitUntil: 'domcontentloaded' });
  23  |   await page.locator(SELECTORS.newsItem).first().waitFor({ timeout: 15000 });
  24  |   const count = await page.locator(SELECTORS.newsItem).count();
  25  |   expect(count, 'Раздел новостей пуст').toBeGreaterThan(0);
  26  | });
  27  | 
  28  | test('Состав — есть хотя бы 10 игроков @major', async ({ page }) => {
  29  |   await page.goto('/teamroster/', { waitUntil: 'domcontentloaded' });
  30  |   await page.locator(SELECTORS.playerCard).first().waitFor({ timeout: 15000 });
  31  |   const count = await page.locator(SELECTORS.playerCard).count();
  32  |   expect(count, 'В составе команды меньше 10 игроков').toBeGreaterThanOrEqual(10);
  33  | });
  34  | 
  35  | test('Тренеры — есть хотя бы один @major', async ({ page }) => {
  36  |   await page.goto('/coaches/', { waitUntil: 'domcontentloaded' });
  37  |   await page.locator(SELECTORS.staffItem).first().waitFor({ timeout: 15000 });
  38  |   const count = await page.locator(SELECTORS.staffItem).count();
  39  |   expect(count, 'Страница тренеров пуста').toBeGreaterThan(0);
  40  | });
  41  | 
  42  | test('Персонал — есть хотя бы 5 человек @major', async ({ page }) => {
  43  |   await page.goto('/staffs/', { waitUntil: 'domcontentloaded' });
  44  |   await page.locator(SELECTORS.staffItem).first().waitFor({ timeout: 15000 });
  45  |   const count = await page.locator(SELECTORS.staffItem).count();
  46  |   expect(count, 'На странице персонала меньше 5 человек').toBeGreaterThanOrEqual(5);
  47  | });
  48  | 
  49  | test('Руководство — есть хотя бы 2 человека @major', async ({ page }) => {
  50  |   await page.goto('/leaders/', { waitUntil: 'domcontentloaded' });
  51  |   await page.locator(SELECTORS.staffItem).first().waitFor({ timeout: 15000 });
  52  |   const count = await page.locator(SELECTORS.staffItem).count();
  53  |   expect(count, 'На странице руководства меньше 2 человек').toBeGreaterThanOrEqual(2);
  54  | });
  55  | 
  56  | test('Турнирная таблица — содержит Торпедо @major', async ({ page }) => {
  57  |   await page.goto('/season/tournament_tables/', { waitUntil: 'domcontentloaded' });
  58  |   await page.locator('text=/торпедо/i').first().waitFor({ timeout: 15000 });
  59  |   const body = await page.textContent('body');
  60  |   expect(body, 'Торпедо не найден в турнирной таблице').toMatch(/торпедо/i);
  61  | });
  62  | 
  63  | test('Статистика — таблица бомбардиров не пустая @major', async ({ page }) => {
  64  |   await page.goto('/season/statistics/', { waitUntil: 'domcontentloaded' });
  65  |   const statsRows = page.locator('table tbody tr, .statistics__row, [class*="stat"] tr');
  66  |   await statsRows.first().waitFor({ timeout: 15000 });
  67  |   const rows = await statsRows.count();
  68  |   expect(rows, 'Таблица статистики пуста').toBeGreaterThan(0);
  69  | });
  70  | 
  71  | test('Видео — есть хотя бы 10 видео @major', async ({ page }) => {
  72  |   await page.goto('/video/', { waitUntil: 'domcontentloaded' });
  73  |   const videoItems = page.locator('[class*="video"] a, .video-card, .video__item, .list-video__item');
  74  |   await videoItems.first().waitFor({ timeout: 15000 });
  75  |   const count = await videoItems.count();
  76  |   expect(count, 'На странице видео меньше 10 элементов').toBeGreaterThanOrEqual(10);
  77  | });
  78  | 
  79  | test('Фотогалерея — есть хотя бы 1 альбом @major', async ({ page }) => {
  80  |   await page.goto('/photogallery/', { waitUntil: 'domcontentloaded' });
  81  |   const galleryItems = page.locator('[class*="gallery"] a, .gallery__item, .photogallery__item');
  82  |   await galleryItems.first().waitFor({ timeout: 15000 });
  83  |   const count = await galleryItems.count();
  84  |   expect(count, 'Фотогалерея пуста').toBeGreaterThan(0);
  85  | });
  86  | 
  87  | test('Календарь — ячейки содержат текст команд @major', async ({ page }) => {
  88  |   await page.goto('/season/calendar/', { waitUntil: 'domcontentloaded' });
  89  | 
  90  |   const matchCells = page.locator(SELECTORS.matchCell);
  91  |   const count = await matchCells.count();
  92  | 
  93  |   if (count > 0) {
  94  |     // Проверяем что хотя бы одна ячейка содержит текст команды
  95  |     let hasText = false;
  96  |     for (let i = 0; i < Math.min(count, 10); i++) {
  97  |       const text = await matchCells.nth(i).textContent();
  98  |       if (text.trim().length > 0) {
  99  |         hasText = true;
  100 |         break;
  101 |       }
  102 |     }
  103 |     expect(hasText, 'Ни одна ячейка календаря не содержит текста команд').toBeTruthy();
  104 |   }
  105 | });
  106 | 
```