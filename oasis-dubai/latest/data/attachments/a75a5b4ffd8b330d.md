# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: 02-table-pages.spec.ts >> Страница: Размещения >> Поиск фильтрует таблицу @regression
- Location: tests/e2e/specs/02-table-pages.spec.ts:41:11

# Error details

```
Error: expect(locator).toHaveCount(expected) failed

Locator:  locator('tbody tr')
Expected: 1
Received: 0
Timeout:  15000ms

Call log:
  - Expect "toHaveCount" with timeout 15000ms
  - waiting for locator('tbody tr')
    4 × locator resolved to 5 elements
      - unexpected value "5"
    15 × locator resolved to 0 elements
       - unexpected value "0"

```

# Page snapshot

```yaml
- generic [ref=e4]:
  - complementary [ref=e5]:
    - generic [ref=e6]:
      - generic [ref=e7]:
        - link "Oasis One" [ref=e9] [cursor=pointer]:
          - /url: /accommodations
        - button [ref=e10] [cursor=pointer]:
          - img [ref=e11]
      - list [ref=e17]:
        - listitem [ref=e18] [cursor=pointer]:
          - link "Assets" [ref=e19]:
            - /url: /assets
            - img [ref=e21]
            - generic [ref=e27]: Assets
        - listitem [ref=e28] [cursor=pointer]:
          - link "Accommodations" [ref=e29]:
            - /url: /accommodations
            - img [ref=e31]
            - generic [ref=e44]:
              - generic [ref=e45]: Accommodations
              - img [ref=e47]
        - listitem [ref=e49] [cursor=pointer]:
          - link "Food" [ref=e50]:
            - /url: /food
            - img [ref=e52]
            - generic [ref=e62]: Food
        - listitem [ref=e63] [cursor=pointer]:
          - link "Orders" [ref=e64]:
            - /url: /transactions
            - img [ref=e66]
            - generic [ref=e76]: Orders
        - listitem [ref=e77] [cursor=pointer]:
          - link "Employees" [ref=e78]:
            - /url: /user
            - img [ref=e80]
            - generic [ref=e90]: Employees
        - listitem [ref=e91] [cursor=pointer]:
          - link "Tickets" [ref=e92]:
            - /url: /requests
            - img [ref=e94]
            - generic [ref=e104]: Tickets
        - listitem [ref=e105] [cursor=pointer]:
          - link "Feedback" [ref=e106]:
            - /url: /feedback
            - img [ref=e108]
            - generic [ref=e113]: Feedback
        - listitem [ref=e114] [cursor=pointer]:
          - link "News" [ref=e115]:
            - /url: /announcements
            - img [ref=e117]
            - generic [ref=e121]: News
        - listitem [ref=e122] [cursor=pointer]:
          - link "Questionnaires" [ref=e123]:
            - /url: /questionnaires
            - img [ref=e125]
            - generic [ref=e133]: Questionnaires
        - listitem [ref=e134] [cursor=pointer]:
          - link "Events" [ref=e135]:
            - /url: /events
            - img [ref=e137]
            - generic [ref=e145]: Events
        - listitem [ref=e146] [cursor=pointer]:
          - link "Services" [ref=e147]:
            - /url: /services
            - img [ref=e149]
            - generic [ref=e152]: Services
        - listitem [ref=e153] [cursor=pointer]:
          - link "Entertainment" [ref=e154]:
            - /url: /entertainment
            - img [ref=e156]
            - generic [ref=e182]: Entertainment
        - listitem [ref=e183] [cursor=pointer]:
          - link "Loyalty" [ref=e184]:
            - /url: /promotions
            - img [ref=e186]
            - generic [ref=e190]: Loyalty
      - generic [ref=e191]:
        - list [ref=e192]:
          - listitem [ref=e193] [cursor=pointer]:
            - link "Notifications" [ref=e194]:
              - /url: /notifications
              - img [ref=e196]
              - generic [ref=e208]: Notifications
          - listitem [ref=e209] [cursor=pointer]:
            - link "Settings" [ref=e210]:
              - /url: /settings
              - img [ref=e212]
              - generic [ref=e218]: Settings
          - listitem [ref=e219] [cursor=pointer]:
            - link "QR Generator" [ref=e220]:
              - /url: /qr
              - img [ref=e222]
              - generic [ref=e226]: QR Generator
        - button "Log out" [ref=e227] [cursor=pointer]:
          - img [ref=e229]
          - generic [ref=e236]: Log out
  - generic [ref=e237]:
    - banner [ref=e238]:
      - generic [ref=e239]:
        - navigation [ref=e240]:
          - list [ref=e241]:
            - listitem [ref=e242]:
              - link "Dashboard" [ref=e243] [cursor=pointer]:
                - /url: /dashboard
                - img [ref=e244]
                - generic [ref=e249]: Dashboard
            - listitem [ref=e250]:
              - link "Knowledge base" [ref=e251] [cursor=pointer]:
                - /url: /knowledge-base
                - img [ref=e252]
                - generic [ref=e255]: Knowledge base
            - listitem [ref=e256]:
              - link "Storage" [ref=e257] [cursor=pointer]:
                - /url: /documents
                - img [ref=e258]
                - generic [ref=e263]: Storage
        - button "A" [ref=e265] [cursor=pointer]:
          - generic [ref=e267]: A
    - main [ref=e269]:
      - generic [ref=e271]:
        - generic [ref=e274]:
          - heading "Accommodation facilities" [level=2] [ref=e277]
          - generic [ref=e278]:
            - link "Add new object" [ref=e279] [cursor=pointer]:
              - /url: /accommodations/create
            - button "Placements analytics" [ref=e280] [cursor=pointer]:
              - generic [ref=e281]: Placements analytics
              - img [ref=e283]
        - generic [ref=e293]:
          - generic [ref=e294]:
            - generic [ref=e295]:
              - generic [ref=e296]: Show
              - generic [ref=e298] [cursor=pointer]:
                - generic [ref=e299]:
                  - combobox [ref=e301]
                  - generic "15" [ref=e302]
                - generic:
                  - img:
                    - img
              - generic [ref=e303]: entries
            - generic [ref=e306]:
              - generic [ref=e308]: "Search:"
              - textbox "Search:" [active] [ref=e309]:
                - /placeholder: Search by ID, N.P.
                - text: zzz_e2e_no_match_99999
          - table [ref=e311]:
            - rowgroup [ref=e312]:
              - row "Name Section Location Asset floors rooms sleeping places Action" [ref=e313]:
                - columnheader [ref=e314]:
                  - heading [level=4] [ref=e317] [cursor=pointer]:
                    - checkbox [ref=e319]
                - columnheader "Name" [ref=e321]:
                  - generic [ref=e323] [cursor=pointer]:
                    - heading "Name" [level=4] [ref=e324]
                    - generic [ref=e325]:
                      - button [ref=e326]:
                        - img [ref=e327]
                      - button [ref=e330]:
                        - img [ref=e331]
                - columnheader "Section" [ref=e334]:
                  - generic [ref=e336] [cursor=pointer]:
                    - heading "Section" [level=4] [ref=e337]
                    - generic [ref=e338]:
                      - button [ref=e339]:
                        - img [ref=e340]
                      - button [ref=e343]:
                        - img [ref=e344]
                - columnheader "Location" [ref=e347]:
                  - generic [ref=e349] [cursor=pointer]:
                    - heading "Location" [level=4] [ref=e350]
                    - generic [ref=e351]:
                      - button [ref=e352]:
                        - img [ref=e353]
                      - button [ref=e356]:
                        - img [ref=e357]
                - columnheader "Asset" [ref=e360]:
                  - generic [ref=e362] [cursor=pointer]:
                    - heading "Asset" [level=4] [ref=e363]
                    - generic [ref=e364]:
                      - button [ref=e365]:
                        - img [ref=e366]
                      - button [ref=e369]:
                        - img [ref=e370]
                - columnheader "floors" [ref=e373]:
                  - generic [ref=e375] [cursor=pointer]:
                    - heading "floors" [level=4] [ref=e376]
                    - generic [ref=e377]:
                      - button [ref=e378]:
                        - img [ref=e379]
                      - button [ref=e382]:
                        - img [ref=e383]
                - columnheader "rooms" [ref=e386]:
                  - generic [ref=e388] [cursor=pointer]:
                    - heading "rooms" [level=4] [ref=e389]
                    - generic [ref=e390]:
                      - button [ref=e391]:
                        - img [ref=e392]
                      - button [ref=e395]:
                        - img [ref=e396]
                - columnheader "sleeping places" [ref=e399]:
                  - generic [ref=e401] [cursor=pointer]:
                    - heading "sleeping places" [level=4] [ref=e402]
                    - generic [ref=e403]:
                      - button [ref=e404]:
                        - img [ref=e405]
                      - button [ref=e408]:
                        - img [ref=e409]
                - columnheader "Action" [ref=e412]:
                  - heading "Action" [level=4] [ref=e415] [cursor=pointer]
            - rowgroup
          - navigation "Pagination" [ref=e418]:
            - listitem [ref=e419]:
              - button "Previous page" [disabled] [ref=e420] [cursor=pointer]: Prev
            - listitem [ref=e421]:
              - button "Page 1 is your current page" [ref=e422] [cursor=pointer]: "1"
            - listitem [ref=e423]:
              - button "Next page" [disabled] [ref=e424] [cursor=pointer]: next
```

# Test source

```ts
  1  | import { test, expect } from '@playwright/test';
  2  | import { TablePage } from '../pages/table.page';
  3  | import { waitForTableData } from '../helpers/wait.helper';
  4  | 
  5  | const tablePages = [
  6  |   { name: 'Размещения', url: '/accommodations', hasSearch: true },
  7  |   { name: 'Сотрудники', url: '/user', hasSearch: true },
  8  |   { name: 'Точки питания', url: '/food', hasSearch: true },
  9  |   { name: 'Заказы', url: '/transactions', hasSearch: false },
  10 | ];
  11 | 
  12 | for (const p of tablePages) {
  13 |   test.describe(`Страница: ${p.name}`, () => {
  14 |     let tablePage: TablePage;
  15 | 
  16 |     test.beforeEach(async ({ page }) => {
  17 |       tablePage = new TablePage(page, p.url);
  18 |       await tablePage.goto();
  19 |     });
  20 | 
  21 |     test('Таблица загружается @smoke', async ({ page }) => {
  22 |       const hasData = await waitForTableData(page);
  23 | 
  24 |       // Check that page content loaded (heading, table headers, or error message)
  25 |       const pageContent = page.locator('h2, h1, thead th, [class*="error"], [class*="Error"]');
  26 |       await expect(pageContent.first()).toBeVisible({ timeout: 15000 });
  27 | 
  28 |       // Check for server error on the page
  29 |       const errorBanner = page.locator('text=/error has occurred|ошибка/i');
  30 |       const hasError = await errorBanner.count() > 0;
  31 |       if (hasError) {
  32 |         console.warn(`[${p.name}] Page shows a server error — backend issue, not a frontend bug`);
  33 |       }
  34 | 
  35 |       if (!hasData && !hasError) {
  36 |         console.warn(`[${p.name}] Table loaded but has no data rows`);
  37 |       }
  38 |     });
  39 | 
  40 |     if (p.hasSearch) {
  41 |       test('Поиск фильтрует таблицу @regression', async ({ page }) => {
  42 |         const hasData = await waitForTableData(page);
  43 |         if (!hasData) {
  44 |           test.skip(true, `${p.name} table has no data — cannot test search`);
  45 |           return;
  46 |         }
  47 | 
  48 |         await expect(tablePage.searchInput).toBeVisible({ timeout: 10000 });
  49 | 
  50 |         await tablePage.search('zzz_e2e_no_match_99999');
  51 | 
  52 |         // Table shows "No data" — 1 placeholder row, or 0 rows
> 53 |         await expect(page.locator('tbody tr')).toHaveCount(1, { timeout: 15000 });
     |                                                ^ Error: expect(locator).toHaveCount(expected) failed
  54 |       });
  55 |     }
  56 |   });
  57 | }
  58 | 
```