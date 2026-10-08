# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: working_with-apis.spec.ts >> has title
- Location: tests\working_with-apis.spec.ts:26:5

# Error details

```
TypeError: 'caller', 'callee', and 'arguments' properties may not be accessed on strict mode functions or the arguments objects for calls to them
```

```
Error: expect(locator).toContainText(expected) failed

Locator: locator('.preview-link h1').first()
Expected substring: "This is MOCK title for the article"
Error: element(s) not found

Call log:
  - Expect "toContainText" locator('.preview-link h1').first() with timeout 5000ms
  - waiting for locator('.preview-link h1').first()
  - operation was aborted: Test ended.

```

```yaml
- navigation:
  - link "conduit":
    - /url: /
  - list:
    - listitem:
      - link "Home":
        - /url: /
    - listitem:
      - link "Sign in":
        - /url: /login
    - listitem:
      - link "Sign up":
        - /url: /register
- heading "conduit" [level=1]
- paragraph: A place to learn and practice test automation.
- paragraph:
  - link "https://bondaracademy.com":
    - /url: https://bondaracademy.com?utm_source=conduitapp&utm_medium=subdomain&utm_campaign=subdomain_traffic
- list:
  - listitem: Your Feed
  - listitem: Global Feed
- text: Loading articles...
- paragraph: Popular Tags
- text: Automation Playwright
- contentinfo:
  - link "conduit":
    - /url: /
  - text: © 2026. An interactive learning project from
  - link "RealWorld OSS Project":
    - /url: https://github.com/gothinkster/realworld
  - text: . Code licensed under MIT. Hosted by
  - link "Bondar Academy":
    - /url: https://bondaracademy.com
  - text: .
```

# Test source

```ts
  1  | import { test, expect } from '@playwright/test';
  2  | import tags from '../test-data/tags.json'
  3  | 
  4  | test.beforeEach(async ({ page }) => {
  5  |   await page.route('*/**/api/tags', async route => {
  6  |     await route.fulfill({
  7  |       json: tags
  8  |     })
  9  | 
  10 |   })
  11 |   await page.route('*/**/api/articles*', async route => {
  12 |     const response = await route.fetch()
  13 |     const responseJSON = await response.json
  14 |     responseJSON.arguments[0].title = 'This is MOCK title for the article'
  15 |     //responseJSON.articles[0].description = 'This is MOCK description for the article'
  16 |     await route.fulfill({
  17 |       json: responseJSON
  18 |     })
  19 |   })
  20 | 
  21 | 
  22 |   await page.goto('https://conduit.bondaracademy.com/');
  23 | 
  24 | });
  25 | 
  26 | test('has title', async ({ page }) => {
  27 |   await expect(page.locator('.navbar-brand')).toHaveText(/conduit/);
  28 |   await expect(page.locator('.sidebar .tag-pill')).toContainText(['Automation', 'Playwright'])
> 29 |   await expect(page.locator('.preview-link h1').first()).toContainText('This is MOCK title for the article')
     |                                                          ^ Error: expect(locator).toContainText(expected) failed
  30 |   //await expect(page.locator('.preview-link p').first()).toContainText('This is MOCK description for the article')
  31 | });
  32 | 
  33 | 
  34 | 
```