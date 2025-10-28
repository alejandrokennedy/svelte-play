# ACS/C&EN Svelte Starter

**This is a downstream fork** of [The Pudding's Svelte Starter](https://github.com/the-pudding/svelte-starter), customized for American Chemical Society (ACS) and Chemical & Engineering News (C&EN) data journalism and visualization projects.

## Fork Maintenance Strategy

This fork uses a **Feature Branch Strategy** to stay synchronized with upstream changes:

- **`main` branch**: Clean mirror of upstream for easy merging
- **`acs-main` branch**: ACS/C&EN customizations (branding, chemistry-specific features)
- **Upstream sync**: Regular merging from The Pudding's repository to stay current with Svelte 5 migration and new features

### Step-by-Step Upstream Sync Process

#### 1. Update Main Branch with Upstream
```bash
# Fetch latest changes from upstream
git fetch upstream

# Switch to main branch and merge upstream changes
git checkout main
git merge upstream/main

# Push updated main to origin
git push origin main
```

#### 2. Safely Merge into ACS Branch
```bash
# Switch to acs-main and create a backup
git checkout acs-main
git branch acs-main-backup-$(date +%Y%m%d)

# Check what's coming in from main
git diff acs-main..main --stat
```

#### 3. Conservative Merge with Visual Conflict Resolution
```bash
# Initiate merge (this will create conflicts for manual control)
git merge main --no-ff
```

#### 4. Use Zed's Git Tools for Visual Resolution
- Open **Git Panel**: `git panel: toggle focus` or click Git icon
- Open **Project Diff**: `Ctrl+G D` / `Cmd+G D` for visual conflict resolution
- For each conflict:
  - **Prioritize acs-main changes** (logo, branding, config)
  - **Accept main's updates** for packages and dependencies
  - **Edit directly** in Project Diff multibuffer view
  - **Stage resolved hunks** with `Cmd+Y` / `Alt+Y`

#### 5. Handle Special Cases
- **Package files**: Accept main's versions entirely with `git checkout --theirs package.json package-lock.json`
- **Logo/branding**: Keep acs-main versions (your customizations)
- **pnpm-lock.yaml**: Keep deleted if gitignored, or `git rm pnpm-lock.yaml`

#### 6. Complete the Merge
```bash
# Verify all conflicts resolved
git status

# Commit the merge
git commit -m "Merge main branch updates, keeping acs-main customizations"

# Verify important files are intact
ls src/svg/  # Check logo files
git log --oneline -5  # Review merge history
```

### Merge Strategy Philosophy
- **ACS customizations take precedence**: Logo, branding, chemistry-specific features
- **Accept beneficial upstream updates**: Package versions, security updates, new features
- **Conservative approach**: Always create backups, resolve conflicts manually
- **Visual tools**: Use Zed's Project Diff for clear conflict resolution

---

## About the Original Template

# Svelte Starter

**NOTE**: This uses Svelte 5 and is under active migration (not all features will work). For the less adventurous, use the [previous version](https://github.com/the-pudding/svelte-starter) (with Svelte 4).

This [starter template](https://github.com/the-pudding/svelte-starter) aims to quickly scaffold a [SvelteKit](https://kit.svelte.dev/) project, designed around data-driven, visual stories at [The Pudding](https://pudding.cool).

### Notes
* _Do not use or reproduce The Pudding logos or fonts without written permission._
* _Prettier Formatting: Disable any text editor Prettier extensions to take advantage of the built-in rules._

### Features

- [ArchieML](http://archieml.org/) for micro-CMS powered by Google Docs and Sheets
- [Lucide Icons](https://lucide.dev/) for simple/easy svg icons
- [Style Dictionary](https://amzn.github.io/style-dictionary/) for CSS/JS style parity
- [Runed](https://runed.dev/docs) for svelte5 rune utilities
- CSV, JSON, and SVG imports
- SSR static-hosted builds by default

## Quickstart
#### From Scratch
* Click the green `Use this template` button above.
* Alternatively: `npx degit the-pudding/svelte-starter my-project`

#### Pre-existing Project
* clone the repo

#### Installation
* In your local repo run `pnpm install` or `npm install`

## Development

```bash
npm run dev
```

Change the script in `package.json` to `"dev": "svelte-kit dev --host"` to test on your local network on a different device.

## Deploy
Check out the `Makefile` for specific tasks.

### Staging (on Github)
```bash
npm run staging
```

### Production (on AWS for pudding.cool)
```bash
npm run prodution
```

### Manual
```bash
npm run build
```
This generates a directory called `build` with the statically rendered app.

### Password-Protected
To create a password-protected build:

Make sure you have a `.env` file in your root with a value of `PASSWORD=yourpassword`
```bash
make protect
```

Then run either `make github` or `make pudding`.

## Style

There are a few stylesheets included by default in `src/styles`. Refer to them in `app.css`, the place for applying global styles.

For variable parity in both CSS and JS, modify files in the `properties` folder using the [Style Dictionary](https://amzn.github.io/style-dictionary/) API.

Run `npm run style` to regenerate the style dictionary.

#### Some css utility classes in reset.css
* `.sr-only`: makes content invisible available for screen reader
* `.text-outline`: adds a psuedo stroke to text element

### Custom Fonts
For locally hosted fonts, simply add the font to the `static/assets` folder and include a reference in `src/styles/font.css`, making sure the url starts with `"assets/..."`.

## Google Docs and Sheets

* Create a Google Doc or Sheet
* Click `Share` -> `Advanced` -> `Change...` -> `Anyone with this link`
* In the address bar, grab the ID - eg. "...com/document/d/**1IiA5a5iCjbjOYvZVgPcjGzMy5PyfCzpPF-LnQdCdFI0**/edit"
* paste in the ID above into `google.config.js`, and set the filepath to where you want the file saved
* If you want to do a Google Sheet, be sure to include the `gid` value in the url as well

Running `npm run gdoc` at any point (even in new tab while server is running) will fetch the latest from all Docs and Sheets.

## Structural Overview

### Pages
The `src/routes` directory contains pages for your app. For a single-page app (most cases) you don't have to modify anything in here. `+page.svelte` represents the root page, think of it as the `index.html` file. It is prepopulated with a few things like metadata and font preloading. It also includes a reference to a blank slate component `src/components/Index.svelte`. This is the file you want to really start in for your app.

### Embedding Data
For smaller datasets, it is often great to embed the data into the HTML file. If you want to use data as-is, you can use normal import syntax (e.g., `import data from "$data/file.csv"`). If you are working with data but you want to preserve the original or clean/parse just what you need to use in the browser to optimize the front-end payload, you can load it via `+page.server.js`, do some work on it, and return just what you need. This is passed automatically to `+page.svelte` and accessible in any component with `getContext("data")`.


## Pre-loaded helpers

### Components

Located in `src/components`.

```js
// Usage
import Example from "$components/Example.svelte";
```

* `Footer.svelte`: Pudding recirculation and social links.
* `Header.svelte`: Pudding masthead.

### Helper Components

Located in `src/components/helpers`.

```js
// Usage
import Example from "$components/helpers/Example.svelte";
```

*Available*
* `Scrolly.svelte`: Scrollytelling.

*Need to migrate*
* `ButtonSet.svelte`: Accessible button group inputs.
* `Chunk.svelte`: Split text into smaller dom element chunks.
* `Countdown.svelte`: Countdown timer text.
* `DarkModeToggle.svelte`: A toggle button for dark mode.
* `Figure.svelte`: A barebones chart figure component to handle slots.
* `MotionToggle.svelte`: A toggle button to enable/disable front-end user motion preference.
* `Range.svelte`: Customizable range slider.
* `ShareLink.svelte`: Button to share link natively/copy to clipboard.
* `SortTable.svelte`: Sortable semantic table with customizable props.
* `Slider.svelte (and Slider.Slide.svelte)`: A slider widget, especially useful for swipe/slide stories.
* `Tap.svelte`: Edge-of-screen tapping library, designed to integrate with slider.
* `Tip.svelte`: Button that links to Strip payment link.
* `Toggle.svelte`: Accessible toggle inputs.

### Headless Components

[bits UI](https://www.bits-ui.com/docs/introduction) comes pre-installed. It is recommended to use these for any UI components.

### Layercake Chart Components

Starter templates for various chart types to be used with [LayerCake](https://layercake.graphics/). Located in `src/components/layercake`.

*Note:* You must install the module `layercake` first.

```js
// Usage
import Example from "$components/layercake/Example.svelte";
```

### Actions

Located in `src/actions`.

```js
// Usage
import example from "$actions/action.js";
```

* `canTab.js`: enable/disable tabbing on child elements.
* `checkOverlap.js`: Label overlapping detection. Loops through selection of nodes and adds a class to the ones that are overlapping. Once one is hidden it ignores it.
* `focusTrap.js`: Enable a keyboard focus trap for modals and menus.
* `keepWithinBox.js`: Offsets and element left/right to stay within parent.
* `inView.js`: detect when an element enters or exits the viewport.
* `resize.js`: detect when an element is resized.

### Runes

These are located in `src/runes`. You can put custom ones in `src/runes/misc.js` or create unique files for more complex ones.

```js
import { example } from "$runes/misc/misc.js";
```

* `useWindowDimensions`: returns an object `{ width, height }` of the viewport dimensions. It is debounced for performance.
* `useClipboard`: copy content to clipboard.
* `useFetcher`: load async data from endpoints (local or external).
* `useWindowFocus`: determine if the window is in focus or not.

For more preset runes, use [runed](https://runed.dev/docs) which is preloaded.

### Utils

Located in `src/utils/`.

```js
// Usage
import example from "$utils/example.js";
```
* `checkScrollDir.js`: Gets the user's scroll direction ("up" or "down")
* `csvDownload.js`: Converts a flat array of data to CSV content ready to be used as an `href` value for download.
* `generateId.js`: Generate an alphanumeric id.
* `loadCsv.js`: Loads and parses a CSV file.
* `loadImage.js`: Loads an image.
* `loadJson.js`: Loads and parses a JSON file.
* `loadPixels.js`: Loads the pixel data of an image via an offscreen canvas.
* `localStorage.js`: Read and write to local storage.
* `mapToArray.js`: Convenience function to convert a map to an array.
* `move.js`: transform translate function shorthand.
* `transformSvg.js`: Custom transition lets you apply an svg transform property with the in/out svelte transition. Parameters (with defaults):
* `translate.js`: Convenience function for transform translate css.
* `urlParams.js`: Get and set url parameters.

## Tips

### Image asset paths
For `img` tags, use relative paths:

```html
<img src="assets/demo/test.jpg" />
```

or use `base` if on a sub route:

```html
<script>
	import { base } from "$app/paths";
</script>

<img src="{base}/assets/demo/test.jpg"  />
```

For CSS background images, use absolute paths:

```css
background: url("/assets/demo/test.jpg");
```

View example code in the preloaded demo.
