# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 12-next-match.spec.js >> Infobar (sticky) — данные совпадают с блоком «Следующий матч» @major
- Location: tests/e2e/12-next-match.spec.js:57:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/
Call log:
  - navigating to "https://dev.hctorpedo.ru/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: блок «Следующий матч» и infobar на главной.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | import { SELECTORS, KHL_TEAMS_PATTERN } from './config.js';
  6  | 
  7  | test('Блок «Следующий матч» — отображается или скрыт без ошибок @critical', async ({ page }) => {
  8  |   const errors = [];
  9  |   page.on('pageerror', (err) => errors.push(err.message));
  10 | 
  11 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
  12 |   await page.waitForLoadState('networkidle', { timeout: 15000 }).catch(() => {});
  13 | 
  14 |   const matchBlock = page.locator(SELECTORS.matchBlock).first();
  15 |   const isVisible = await matchBlock.isVisible().catch(() => false);
  16 | 
  17 |   if (isVisible) {
  18 |     const text = await matchBlock.textContent();
  19 |     // Должен содержать хотя бы одно название команды или дату
  20 |     expect(
  21 |       text.trim().length,
  22 |       'Блок «Следующий матч» виден, но пустой'
  23 |     ).toBeGreaterThan(5);
  24 |   }
  25 | 
  26 |   // Независимо от видимости — не должно быть JS-ошибок
  27 |   expect(
  28 |     errors,
  29 |     `JS-ошибки при загрузке блока «Следующий матч»:\n${errors.join('\n')}`
  30 |   ).toHaveLength(0);
  31 | });
  32 | 
  33 | test('Блок «Следующий матч» — логотипы команд загружены @major', async ({ page }) => {
  34 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
  35 |   await page.waitForLoadState('networkidle', { timeout: 15000 }).catch(() => {});
  36 | 
  37 |   const matchBlock = page.locator(SELECTORS.matchBlock).first();
  38 |   if (!(await matchBlock.isVisible().catch(() => false))) {
  39 |     test.skip();
  40 |     return;
  41 |   }
  42 | 
  43 |   const images = await matchBlock.locator('img').evaluateAll((imgs) =>
  44 |     imgs.filter((img) => img.src && !img.src.includes('data:')).map((img) => ({
  45 |       src: img.src,
  46 |       loaded: img.naturalWidth > 0,
  47 |     }))
  48 |   );
  49 | 
  50 |   const broken = images.filter((img) => !img.loaded);
  51 |   expect(
  52 |     broken,
  53 |     `Битые логотипы в блоке матча:\n${broken.map((b) => b.src).join('\n')}`
  54 |   ).toHaveLength(0);
  55 | });
  56 | 
  57 | test('Infobar (sticky) — данные совпадают с блоком «Следующий матч» @major', async ({ page }) => {
> 58 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev.hctorpedo.ru/
  59 |   await page.waitForLoadState('networkidle', { timeout: 15000 }).catch(() => {});
  60 | 
  61 |   const matchBlock = page.locator(SELECTORS.matchBlock).first();
  62 |   if (!(await matchBlock.isVisible().catch(() => false))) {
  63 |     test.skip();
  64 |     return;
  65 |   }
  66 | 
  67 |   const matchText = await matchBlock.textContent().catch(() => '');
  68 | 
  69 |   // Скроллим вниз чтобы infobar появился
  70 |   await page.evaluate(() => window.scrollTo(0, document.body.scrollHeight));
  71 |   const infobar = page.locator('#infobar, [class*="infobar"]').first();
  72 |   if (!(await infobar.isVisible().catch(() => false))) {
  73 |     test.skip();
  74 |     return;
  75 |   }
  76 | 
  77 |   const infobarText = await infobar.textContent().catch(() => '');
  78 | 
  79 |   // Извлекаем названия команд из обоих блоков — должны совпадать
  80 |   const teamPattern = KHL_TEAMS_PATTERN;
  81 |   const matchTeams = (matchText.match(teamPattern) || []).map((t) => t.toLowerCase()).sort();
  82 |   const infobarTeams = (infobarText.match(teamPattern) || []).map((t) => t.toLowerCase()).sort();
  83 | 
  84 |   if (matchTeams.length > 0 && infobarTeams.length > 0) {
  85 |     expect(infobarTeams, 'Команды в infobar не совпадают с блоком матча').toEqual(matchTeams);
  86 |   }
  87 | });
  88 | 
```