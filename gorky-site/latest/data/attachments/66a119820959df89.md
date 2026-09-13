# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 03-functional.spec.js >> Модалка авторизации открывается @critical
- Location: tests/e2e/03-functional.spec.js:14:5

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/
Call log:
  - navigating to "https://dev-gorky.hctorpedo.ru/", waiting until "domcontentloaded"

```

# Test source

```ts
  1  | /**
  2  |  * Smoke: ключевые функции работают.
  3  |  */
  4  | import { test, expect } from '@playwright/test';
  5  | 
  6  | test('Страница /tickets/ открывается и содержит контент @critical', async ({ page }) => {
  7  |   const resp = await page.goto('/tickets/', { waitUntil: 'domcontentloaded' });
  8  |   expect(resp.status(), '/tickets/ вернул ' + resp.status()).toBe(200);
  9  | 
  10 |   const body = await page.textContent('body');
  11 |   expect(body.toLowerCase()).toMatch(/билет|ticket/);
  12 | });
  13 | 
  14 | test('Модалка авторизации открывается @critical', async ({ page }) => {
> 15 |   await page.goto('/', { waitUntil: 'domcontentloaded' });
     |              ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://dev-gorky.hctorpedo.ru/
  16 | 
  17 |   // На мобилке "Войти" скрыт в бургер-меню — открываем его
  18 |   let loginLink = page.locator('a.panel__link-auth:visible, a:visible:has-text("Войти")').first();
  19 |   if (!(await loginLink.isVisible().catch(() => false))) {
  20 |     const burger = page.locator('button.header__nav-btn').first();
  21 |     if (await burger.isVisible().catch(() => false)) {
  22 |       await burger.click();
  23 |       // Ждём анимацию slide-in меню
  24 |       const mobileLogin = page.locator('a.mobile-nav__login');
  25 |       await expect(mobileLogin).toBeVisible({ timeout: 5000 });
  26 |       loginLink = mobileLogin;
  27 |     }
  28 |   }
  29 | 
  30 |   await loginLink.dispatchEvent('click');
  31 | 
  32 |   // Проверяем что появилось поле пароля (признак формы авторизации)
  33 |   const passField = page.locator('input[type="password"]:visible').first();
  34 |   await expect(passField, 'Форма авторизации не появилась после клика «Войти»').toBeVisible({ timeout: 5000 });
  35 | });
  36 | 
```