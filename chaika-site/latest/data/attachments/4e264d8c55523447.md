# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 11-ssl-meta.spec.js >> Нет редиректа HTTPS → HTTP @critical
- Location: tests/e2e/11-ssl-meta.spec.js:14:5

# Error details

```
Error: HTTPS недоступен — невозможно проверить редирект: https://dev-chaika.hctorpedo.ru

expect(received).not.toBeNull()

Received: null
```

# Test source

```ts
  1  | /**
  2  |  * Smoke: SSL/HTTPS, мета-теги, favicon.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | test('Сайт доступен по HTTPS @critical', async ({ request }) => {
  7  |   const baseUrl = test.info().project.use.baseURL;
  8  |   const httpsUrl = baseUrl.replace('http://', 'https://');
  9  |   const resp = await request.get(httpsUrl, { maxRedirects: 0 }).catch(() => null);
  10 |   // Должен быть либо 200, либо редирект (301/302) но НЕ ошибка
  11 |   expect(resp, `HTTPS недоступен: ${httpsUrl}`).not.toBeNull();
  12 | });
  13 | 
  14 | test('Нет редиректа HTTPS → HTTP @critical', async ({ request }) => {
  15 |   const baseUrl = test.info().project.use.baseURL;
  16 |   const httpsUrl = baseUrl.replace('http://', 'https://');
  17 |   const resp = await request.get(httpsUrl, { maxRedirects: 0 }).catch(() => null);
> 18 |   expect(resp, `HTTPS недоступен — невозможно проверить редирект: ${httpsUrl}`).not.toBeNull();
     |                                                                                     ^ Error: HTTPS недоступен — невозможно проверить редирект: https://dev-chaika.hctorpedo.ru
  19 |   const location = resp.headers()['location'] || '';
  20 |   expect(
  21 |     location,
  22 |     `HTTPS редиректит на HTTP: ${location}`
  23 |   ).not.toMatch(/^http:\/\//);
  24 | });
  25 | 
  26 | const META_PAGES = [
  27 |   { path: '/', name: 'Главная' },
  28 |   { path: '/news/', name: 'Новости' },
  29 |   { path: '/teamroster/', name: 'Состав' },
  30 | ];
  31 | 
  32 | for (const pg of META_PAGES) {
  33 |   test(`${pg.name} — <title> заполнен @major`, async ({ page }) => {
  34 |     await page.goto(pg.path, { waitUntil: 'domcontentloaded' });
  35 |     const title = await page.title();
  36 |     expect(title.trim().length, `${pg.path}: пустой <title>`).toBeGreaterThan(0);
  37 |   });
  38 | }
  39 | 
  40 | test('Favicon загружается @minor', async ({ page }) => {
  41 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
  42 | 
  43 |   const faviconHref = await page.locator('link[rel*="icon"]').first().getAttribute('href').catch(() => null);
  44 | 
  45 |   if (!faviconHref) {
  46 |     // Проверяем /favicon.ico по умолчанию
  47 |     const resp = await page.goto('/favicon.ico');
  48 |     expect(resp.status(), 'favicon.ico не найден').not.toBe(404);
  49 |     return;
  50 |   }
  51 | 
  52 |   const resp = await page.goto(faviconHref);
  53 |   expect(resp.status(), `Favicon ${faviconHref} не загрузился`).not.toBe(404);
  54 | });
  55 | 
```