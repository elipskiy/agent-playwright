# Native integration for [Playwright](https://github.com/microsoft/playwright) with [Visual Regression Tracker](https://github.com/Visual-Regression-Tracker/Visual-Regression-Tracker)

## Install

`npm install @visual-regression-tracker/agent-playwright`

## Usage
### Import
```
import {
  PlaywrightVisualRegressionTracker,
  Config,
} from "@visual-regression-tracker/agent-playwright";
import { chromium, Browser, Page, BrowserContext } from "playwright";
```
### Configure connection
```
const browserType = chromium; // any BrowserType supported by Playwright

const config: Config = {
    // Fill with your data
    apiUrl: "http://localhost:4200",

    // Fill with your data
    branchName: "develop",

    // Fill with your data
    projectId: "76f0c443-9811-4f4f-b1c2-7c01c5775d9a",

    // Fill with your data
    apiKey: "F5Z2H0H2SNMXZVHX0EA4YQM1MGDD",
};

const vrt = new PlaywrightVisualRegressionTracker(config, browserType);
```
### Navigate to needed page
```
// set up Playwright 
const browser = await browserType.launch({ headless: false });
const context = await browser.newContext();
const page = await context.newPage();

// navigate to url
await page.goto("https://google.com/");
```
### Send image
```
await vrt.track(page, imageName[, options])
```
* `page` <[Page](https://playwright.dev/#version=v1.0.2&path=docs%2Fapi.md&q=class-page)> Playwright page
* `imageName` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> name for the taken screenshot image
* `options` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> optional configuration with:
* * `diffTollerancePercent` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> specify acceptable difference from baseline, between `0-100`. Default `1`
* * `screenshotOptions` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> configuration for Playwrights `screenshot` method
* * * `fullPage` <[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Boolean_type)> When true, takes a screenshot of the full scrollable page, instead of the currently visibvle viewport. Defaults to `false`.
* * * `omitBackground` <[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Boolean_type)> Hides default white background and allows capturing screenshots with transparency. Defaults to `false`.
* * * `clip` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> An object which specifies clipping of the resulting image. Should have the following fields:
* * * * `x` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> x-coordinate of top-left corner of clip area
* * * * `y` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> y-coordinate of top-left corner of clip area
* * * * `width` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> width of clipping area
* * * * `height` <[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)> height of clipping area
* * `agent` <[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)> Additional information to mark baseline across agents that have different:
* * * `os` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> operating system name, like Windows, Mac, etc.
* * * `device` <[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)> device name, PC identifier, mobile identifier etc.