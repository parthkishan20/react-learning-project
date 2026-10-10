# TRVL

A travel-agency landing page built **for learning purposes**, to practice the React ecosystem of its time:

- **React 17** (function components and hooks)
- **React Router v5** (`<Switch>` / `<Route component>`)
- **Create React App 4** (`react-scripts` 4.0.3)

It is a static front-end with no backend. The pages are Home, Services, Products and Sign Up.

## Getting started

```bash
npm install
npm start      # http://localhost:3000/trvl
```

On Node 17 or newer, react-scripts 4 needs the legacy OpenSSL provider:

```bash
NODE_OPTIONS=--openssl-legacy-provider npm start
```

## Scripts

| Command          | Description                                   |
| ---------------- | --------------------------------------------- |
| `npm start`      | Start the dev server                          |
| `npm run build`  | Create a production build in `build/`         |
| `npm test`       | Run tests in watch mode                       |
| `npm run deploy` | Build and publish to GitHub Pages (`gh-pages`) |
