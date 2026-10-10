# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

"TRVL" — a static travel-agency landing site built with Create React App (`react-scripts` 4.0.3), React 17, and `react-router-dom` v5. No backend, no state management library, no TypeScript.

## Commands

```bash
npm start                              # dev server at http://localhost:3000
npm run build                          # production build to /build
npm test                               # Jest in interactive watch mode
npm test -- --watchAll=false           # run tests once (CI-style)
npm test -- src/path/to/File.test.js   # run a single test file
npm test -- -t "test name"             # run tests matching a name
npm run deploy                         # build + publish /build to GitHub Pages via gh-pages
```

- Linting is CRA's built-in ESLint (`react-app` config); warnings show in the `npm start`/`npm run build` output. There is no separate lint script.
- There are currently no test files in the repo.
- `react-scripts` 4 uses webpack 4, which fails on Node 17+ with an OpenSSL `ERR_OSSL_EVP_UNSUPPORTED` error. Prefix `start`/`build`/`test` with `NODE_OPTIONS=--openssl-legacy-provider` (this was needed to run the dev server on Node 23).

## Architecture

- `src/index.js` → `src/App.js`: `App` wraps everything in `BrowserRouter`, renders `Navbar` above a v5 `<Switch>` with routes `/`, `/services`, `/products`, `/sign-up` (pages live in `src/components/pages/`).
- **Footer is rendered per page, not in `App`.** Each page component imports `Footer` from `src/Footer.js` (note: Footer lives at `src/`, not `src/components/`). New pages must include `<Footer />` themselves.
- **`Button` (`src/components/Button.js`) always wraps its `<button>` in a `<Link to='/sign-up'>`**, so every `Button` navigates to `/sign-up` regardless of `onClick`. Styles/sizes are whitelisted via the `STYLES`/`SIZES` arrays (`btn--primary`, `btn--outline`, `btn--test`; `btn--medium`, `btn--large`) and fall back to the first entry.
- `Cards` composes `CardItem`s, passing `src`, `text`, `label` (rendered as a CSS badge via `data-category`), and `path`.
- Styling is plain CSS, one file per component imported directly into it. The non-Home page banners (`.services`, `.products`, `.sign-up`) are styled in `src/App.css`. `Navbar` toggles a mobile menu and hides its button at `window.innerWidth <= 960`.
- Icons (Font Awesome 5, e.g. `<i className='fab fa-typo3' />`) and the PT Sans font are loaded from CDNs in `public/index.html`, not npm.

## Assets and deployment

- Images and videos exist in both `public/` and `src/assets/`. Code imports from `src/assets/` (bundled by webpack, works under any base path); the `public/` copies are effectively unused.
- `package.json` sets `homepage` to `https://parthkishan20.github.io/trvl`, so the app is served from the `/trvl` subpath both on GitHub Pages and in the dev server (`http://localhost:3000/trvl`). `BrowserRouter` has no `basename`, so routes like `/` won't match at `/trvl` unless `basename={process.env.PUBLIC_URL}` is added (or `HashRouter` is used). Deep-link reloads will also 404 on GitHub Pages.
