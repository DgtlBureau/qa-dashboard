# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 04-visual.spec.js >> Логотип Торпедо в header загружен @major
- Location: tests/e2e/04-visual.spec.js:69:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/
Call log:
  - navigating to "https://dev-gorky.hctorpedo.ru/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: картинки загружаются.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | import { scrollPage } from './config.js';
  6  | 
  7  | async function checkImages(page, path) {
  8  |   await page.goto(path, { waitUntil: 'domcontentloaded' });
  9  |   await scrollPage(page);
  10 | 
  11 |   const images = await page.locator('img:visible').evaluateAll((imgs) =>
  12 |     imgs.map((img) => ({
  13 |       src: img.src || img.getAttribute('data-src') || '',
  14 |       loaded: img.naturalWidth > 0,
  15 |     }))
  16 |   );
  17 | 
  18 |   const relevant = images.filter(
  19 |     (img) =>
  20 |       img.src &&
  21 |       !img.src.includes('data:') &&
  22 |       !img.src.includes('svgsprites') &&
  23 |       !img.src.includes('crm.hctorpedo.ru') &&
  24 |       img.src.length > 10
  25 |   );
  26 | 
  27 |   // Дедуплицируем по src (слайдеры дублируют картинки)
  28 |   const seen = new Set();
  29 |   const unique = relevant.filter((img) => {
  30 |     if (seen.has(img.src)) return false;
  31 |     seen.add(img.src);
  32 |     return true;
  33 |   });
  34 | 
  35 |   const broken = unique.filter((img) => !img.loaded);
  36 |   return { broken, total: unique.length };
  37 | }
  38 | 
  39 | test('Изображения загружены на главной @minor', async ({ page }) => {
  40 |   const { broken, total } = await checkImages(page, '/');
  41 |   const loaded = total - broken.length;
  42 |   // Smoke: на главной должно загрузиться хотя бы 10 изображений
  43 |   // (карусели/слайдеры могут не подгрузиться в headless)
  44 |   expect(loaded, `На главной загрузилось ${loaded}/${total} изображений`).toBeGreaterThanOrEqual(10);
  45 | });
  46 | 
  47 | test('Изображения загружены на /teamroster/ @minor', async ({ page }) => {
  48 |   const { broken, total } = await checkImages(page, '/teamroster/');
  49 |   expect(
  50 |     broken,
  51 |     `На /teamroster/ не загрузились ${broken.length}/${total} изображений:\n${broken.map((b) => `  ${b.src}`).join('\n')}`
  52 |   ).toHaveLength(0);
  53 | });
  54 | 
  55 | test('Изображения загружены на /coaches/ @minor', async ({ page }) => {
  56 |   const { broken, total } = await checkImages(page, '/coaches/');
  57 |   expect(
  58 |     broken,
  59 |     `На /coaches/ не загрузились ${broken.length}/${total} изображений:\n${broken.map((b) => `  ${b.src}`).join('\n')}`
  60 |   ).toHaveLength(0);
  61 | });
  62 | 
  63 | test('Изображения загружены на /news/ @minor', async ({ page }) => {
  64 |   const { broken, total } = await checkImages(page, '/news/');
  65 |   const loaded = total - broken.length;
  66 |   expect(loaded, `На /news/ загрузилось ${loaded}/${total} изображений`).toBeGreaterThanOrEqual(1);
  67 | });
  68 | 
  69 | test('Логотип Торпедо в header загружен @major', async ({ page }) => {
> 70 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/
  71 | 
  72 |   const logo = page.locator('header img, .header img, .header__logo img').first();
  73 |   if ((await logo.count()) === 0) {
  74 |     // Логотип может быть SVG или background-image
  75 |     const svgLogo = page.locator('header svg, .header svg, .header__logo svg').first();
  76 |     expect(await svgLogo.count(), 'Логотип Торпедо не найден в header').toBeGreaterThan(0);
  77 |     return;
  78 |   }
  79 | 
  80 |   const loaded = await logo.evaluate((img) => img.naturalWidth > 0);
  81 |   expect(loaded, 'Логотип Торпедо в header не загрузился').toBeTruthy();
  82 | });
  83 | 
```