# Native integration for [Playwright](https://github.com/microsoft/playwright) with [Visual Regression Tracker](https://github.com/Visual-Regression-Tracker/Visual-Regression-Tracker)

[![Codacy Badge](https://app.codacy.com/project/badge/Grade/7c9f8095909c4220bff56b66b4cb728d)](https://www.codacy.com/gh/Visual-Regression-Tracker/agent-playwright?utm_source=github.com&utm_medium=referral&utm_content=Visual-Regression-Tracker/agent-playwright&utm_campaign=Badge_Grade)
[![Codacy Badge](https://app.codacy.com/project/badge/Coverage/7c9f8095909c4220bff56b66b4cb728d)](https://www.codacy.com/gh/Visual-Regression-Tracker/agent-playwright?utm_source=github.com&utm_medium=referral&utm_content=Visual-Regression-Tracker/agent-playwright&utm_campaign=Badge_Coverage)

## Npm

https://www.npmjs.com/package/@visual-regression-tracker/agent-playwright

## Install

`npm install @visual-regression-tracker/agent-playwright`

## Usage

### Import

```js
import {
  PlaywrightVisualRegressionTracker,
  Config,
} from "@visual-regression-tracker/agent-playwright";
import { chromium, Browser, Page, BrowserContext } from "@playwright/test";
```

### Configure connection

#### Explicit config from code

```js
const config: Config = {
  // URL where backend is running
  // Required
  apiUrl: "http://localhost:4200",

  // Project name or ID
  // Required
  project: "Default project",

  // User apiKey
  // Required
  apiKey: "tXZVHX0EA4YQM1MGDD",

  // Current git branch
  // Required
  branchName: "develop",

  // Log errors instead of throwing exceptions
  // Optional - default false
  enableSoftAssert: true,

  // Unique ID related to one CI build
  // Optional - default null
  ciBuildId: "SOME_UNIQUE_ID",
};

const browserName = chromium.name();
const vrt = new PlaywrightVisualRegressionTracker(browserName, config);
```

#### Or, as JSON config file `vrt.json`

_Used only if not explicit config provided_
_Is overriden if ENV variables are present_

```json
{
  "apiUrl": "http://localhost:4200",
  "project": "Default project",
  "apiKey": "tXZVHX0EA4YQM1MGDD",
  "ciBuildId": "commit_sha",
  "branchName": "develop",
  "enableSoftAssert": false
}
```

#### Or, as environment variables

_Used only if not explicit config provided_

```
VRT_APIURL="http://localhost:4200"
VRT_PROJECT="Default project"
VRT_APIKEY="tXZVHX0EA4YQM1MGDD"
VRT_CIBUILDID="commit_sha"
VRT_BRANCHNAME="develop"
VRT_ENABLESOFTASSERT=true
```

### Setup

```js
vrt.start();
```

### Teardown

```js
vrt.stop();
```

### Navigate to needed page

```js
// set up Playwright
const browser = await browserType.launch({ headless: false });
const context = await browser.newContext();
const page = await context.newPage();

// navigate to url
await page.goto("https://google.com/");
```

### Track page

```js
await vrt.trackPage(page, imageName[, options])
```

- `page` <[Page](https://playwright.dev/#version=v1.0.2&path=docs%2Fapi.md&q=class-page)> Playwright page
- `imageName` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> name for the taken screenshot image
- `options` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> optional configuration with:
- - `diffTollerancePercent` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> specify acceptable difference from baseline, between `0-100`.
- - `comment` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> comment for test run
- - `ignoreAreas` <[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)<[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)>>
- - - `x` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> X-coordinate relative of left upper corner
- - - `y` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> Y-coordinate relative of left upper corner
- - - `width` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> area width in px
- - - `height` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> area height in px
- - `screenshotOptions` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> configuration for Playwrights `screenshot` method
- - - `fullPage` <[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Boolean_type)> When true, takes a screenshot of the full scrollable page, instead of the currently visibvle viewport. Defaults to `false`.
- - - `omitBackground` <[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Boolean_type)> Hides default white background and allows capturing screenshots with transparency. Defaults to `false`.
- - - `clip` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> An object which specifies clipping of the resulting image. Should have the following fields:
- - - - `x` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> x-coordinate of top-left corner of clip area
- - - - `y` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> y-coordinate of top-left corner of clip area
- - - - `width` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> width of clipping area
- - - - `height` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> height of clipping area
- - - `timeout` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> Maximum time in milliseconds, defaults to 30 seconds, pass 0 to disable timeout.
- - `agent` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> Additional information to mark baseline across agents that have different:
- - - `os` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> operating system name, like Windows, Mac, etc.
- - - `device` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> device name, PC identifier, mobile identifier etc.
- - - `viewport` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> viewport size.
- `retryCount` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> Maximum time to retry screenshot if case of diff

### Track elementHandle

```js
await vrt.trackElementHandle(elementHandle, imageName[, options])
```

- `elementHandle` <[ElementHandle](https://playwright.dev/#version=v1.4.0&path=docs%2Fapi.md&q=class-elementhandle)> Playwright ElementHandle
- `imageName` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> name for the taken screenshot image
- `options` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> optional configuration with:
- - `diffTollerancePercent` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> specify acceptable difference from baseline, between `0-100`.
- - `comment` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> comment for test run
- - `ignoreAreas` <[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)<[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)>>
- - - `x` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> X-coordinate relative of left upper corner
- - - `y` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> Y-coordinate relative of left upper corner
- - - `width` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> area width in px
- - - `height` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> area height in px
- - `screenshotOptions` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> configuration for Playwrights `screenshot` method
- - - `omitBackground` <[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Boolean_type)> Hides default white background and allows capturing screenshots with transparency. Defaults to `false`.
- - - `timeout` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> Maximum time in milliseconds, defaults to 30 seconds, pass 0 to disable timeout.
- - `agent` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> Additional information to mark baseline across agents that have different:
- - - `os` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> operating system name, like Windows, Mac, etc.
- - - `device` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> device name, PC identifier, mobile identifier etc.
- - - `viewport` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> viewport size.
- `retryCount` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> Maximum time to retry screenshot if case of diff
