# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 14-khl-sync.spec.js >> API: расписание сезона доступно @major @khl-sync
- Location: tests/e2e/14-khl-sync.spec.js:79:5

# Error details

```
Error: apiRequestContext.get: getaddrinfo ENOTFOUND dev-chaika.hctorpedo.ru
Call log:
  - → GET https://dev-chaika.hctorpedo.ru/api/v1/khl-schedule
    - user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/147.0.7727.15 Safari/537.36
    - accept: */*
    - accept-encoding: gzip,deflate,br

```

# Test source

```ts
  1   | /**
  2   |  * KHL Sync: проверка актуальности данных через API бэкенда.
  3   |  * Эти тесты проверяют что pipeline синхронизации с КХЛ работает.
  4   |  */
  5   | import { test, expect } from '@playwright/test';
  6   | 
  7   | const API_BASE = '/api/v1';
  8   | 
  9   | test('API: список игроков ≥25 @critical @khl-sync', async ({ request }) => {
  10  |   const resp = await request.get(`${API_BASE}/website/players`);
  11  |   expect(resp.status(), 'API /website/players недоступен').toBe(200);
  12  | 
  13  |   const data = await resp.json();
  14  |   const players = Array.isArray(data) ? data : data?.data || [];
  15  |   expect(
  16  |     players.length,
  17  |     `API вернул ${players.length} игроков (ожидается ≥25)`
  18  |   ).toBeGreaterThanOrEqual(25);
  19  | });
  20  | 
  21  | test('API: турнирная таблица содержит Торпедо @critical @khl-sync', async ({ request }) => {
  22  |   const resp = await request.get(`${API_BASE}/website/standings`);
  23  |   expect(resp.status(), 'API /website/standings недоступен').toBe(200);
  24  | 
  25  |   const data = await resp.json();
  26  |   const json = JSON.stringify(data).toLowerCase();
  27  |   expect(json, 'Торпедо не найден в турнирной таблице API').toContain('торпедо');
  28  | });
  29  | 
  30  | test('API: следующий матч — дата актуальна @critical @khl-sync', async ({ request }) => {
  31  |   const resp = await request.get(`${API_BASE}/khl-next-match`);
  32  |   if (resp.status() === 404 || resp.status() === 204) {
  33  |     // Нет ближайшего матча (межсезонье) — допустимо
  34  |     test.skip();
  35  |     return;
  36  |   }
  37  |   expect(resp.status(), 'API /khl-next-match недоступен').toBe(200);
  38  | 
  39  |   const data = await resp.json();
  40  |   const match = data?.data || data;
  41  |   const dateStr = match?.date || match?.start_at || match?.datetime;
  42  | 
  43  |   if (!dateStr) {
  44  |     test.skip();
  45  |     return;
  46  |   }
  47  | 
  48  |   const matchDate = new Date(dateStr);
  49  |   const now = new Date();
  50  |   const daysDiff = (matchDate.getTime() - now.getTime()) / (1000 * 60 * 60 * 24);
  51  | 
  52  |   // Следующий матч должен быть в будущем или не более 2 дней назад (только что сыгранный)
  53  |   expect(
  54  |     daysDiff,
  55  |     `Следующий матч ${dateStr} — ${Math.abs(Math.floor(daysDiff))} дней назад, данные устарели`
  56  |   ).toBeGreaterThanOrEqual(-2);
  57  | });
  58  | 
  59  | test('API: матчи сезона не пустые @major @khl-sync', async ({ request }) => {
  60  |   const resp = await request.get(`${API_BASE}/khl-matches`);
  61  |   expect(resp.status(), 'API /khl-matches недоступен').toBe(200);
  62  | 
  63  |   const data = await resp.json();
  64  |   const json = JSON.stringify(data);
  65  |   // API может возвращать объект с вложенными массивами по турнирам
  66  |   expect(json.length, 'API /khl-matches вернул пустой ответ').toBeGreaterThan(10);
  67  | });
  68  | 
  69  | test('API: статистика игроков доступна @major @khl-sync', async ({ request }) => {
  70  |   const resp = await request.get(`${API_BASE}/website/player-stats`);
  71  |   expect(resp.status(), 'API /website/player-stats недоступен').toBe(200);
  72  | 
  73  |   const data = await resp.json();
  74  |   const json = JSON.stringify(data);
  75  |   // API может возвращать объект с вложенными данными
  76  |   expect(json.length, 'API /website/player-stats вернул пустой ответ').toBeGreaterThan(10);
  77  | });
  78  | 
  79  | test('API: расписание сезона доступно @major @khl-sync', async ({ request }) => {
> 80  |   const resp = await request.get(`${API_BASE}/khl-schedule`);
      |                              ^ Error: apiRequestContext.get: getaddrinfo ENOTFOUND dev-chaika.hctorpedo.ru
  81  |   expect(resp.status(), 'API /khl-schedule недоступен').toBe(200);
  82  | 
  83  |   const data = await resp.json();
  84  |   const json = JSON.stringify(data);
  85  |   expect(json.length, 'API /khl-schedule вернул пустой ответ').toBeGreaterThan(10);
  86  | });
  87  | 
  88  | test('Новости: количество на dev ≈ prod (отклонение ≤5%) @major @khl-sync', async ({ browser }) => {
  89  |   const ctx = await browser.newContext();
  90  | 
  91  |   // Dev — текущее окружение
  92  |   const devPage = await ctx.newPage();
  93  |   await devPage.goto('/news/', { waitUntil: 'domcontentloaded' });
  94  |   await devPage.locator('[class*="pagination"], [class*="Показано"]').first().waitFor({ timeout: 15000 }).catch(() => {});
  95  |   const devText = await devPage.locator('[class*="pagination"], [class*="Показано"]')
  96  |     .first().textContent().catch(() => '');
  97  |   const devMatch = devText.match(/из\s+([\d\s]+)/);
  98  |   const devTotal = devMatch ? parseInt(devMatch[1].replace(/\s/g, '')) : 0;
  99  | 
  100 |   // Prod
  101 |   const prodPage = await ctx.newPage();
  102 |   await prodPage.goto('https://hctorpedo.ru/news/', { waitUntil: 'domcontentloaded' });
  103 |   await prodPage.locator('[class*="pagination"], [class*="Показано"]').first().waitFor({ timeout: 15000 }).catch(() => {});
  104 |   const prodText = await prodPage.locator('[class*="pagination"], [class*="Показано"]')
  105 |     .first().textContent().catch(() => '');
  106 |   const prodMatch = prodText.match(/из\s+([\d\s]+)/);
  107 |   const prodTotal = prodMatch ? parseInt(prodMatch[1].replace(/\s/g, '')) : 0;
  108 | 
  109 |   await ctx.close();
  110 | 
  111 |   expect(devTotal, 'Не удалось получить количество новостей на dev').toBeGreaterThan(0);
  112 |   expect(prodTotal, 'Не удалось получить количество новостей на prod').toBeGreaterThan(0);
  113 | 
  114 |   const diffPercent = Math.abs(devTotal - prodTotal) / prodTotal * 100;
  115 |   expect(
  116 |     diffPercent,
  117 |     `Dev: ${devTotal}, Prod: ${prodTotal} — разница ${Math.round(diffPercent)}% (${Math.abs(devTotal - prodTotal)} новостей)`
  118 |   ).toBeLessThanOrEqual(5);
  119 | });
  120 | 
  121 | test('API: тренерский штаб ≥5 @major @khl-sync', async ({ request }) => {
  122 |   const resp = await request.get(`${API_BASE}/website/staff`);
  123 |   expect(resp.status(), 'API /website/staff недоступен').toBe(200);
  124 | 
  125 |   const data = await resp.json();
  126 |   const staff = Array.isArray(data) ? data : data?.data || [];
  127 |   expect(
  128 |     staff.length,
  129 |     `API вернул ${staff.length} человек в штабе (ожидается ≥5)`
  130 |   ).toBeGreaterThanOrEqual(5);
  131 | });
  132 | 
```