# A3 Brain Storm — Desktop App

This folder wraps the A3 Brain Storm web app in Electron, so it can be built
into a real installable Windows (.exe) and Mac (.dmg) program.

## Why this can't be built here

Producing an actual .exe or .dmg requires downloading Electron + electron-builder
from npm (several hundred MB) and, for a signed Mac build, running on macOS
with Xcode command line tools. This sandboxed environment has no internet
access, so that download and build step has to happen on your own machine
(or a CI service like GitHub Actions).

## What's in this folder

- `app.html` — the full app (copied from the web/PWA version)
- `main.js` — the Electron shell that opens app.html in a native window
- `package.json` — build configuration (electron-builder) for Windows, Mac and Linux
- `build/icon.png` — the A3 monogram icon

## How to build it yourself

You'll need [Node.js](https://nodejs.org) installed (v18 or newer).

```bash
cd a3-brainstorm-desktop
npm install
npm start          # runs it live, to check it works
npm run build:win  # produces an .exe installer (dist/ folder)
npm run build:mac  # produces a .dmg (must be run on a Mac)
npm run build:linux
```

The finished installer appears in a `dist/` folder afterward.

## Before shipping this for real

- **Icons**: `build/icon.png` is a PNG. For the sharpest result, convert it to
  `icon.ico` (Windows) and `icon.icns` (Mac) — free converters exist online
  (e.g. icoconvert.com) — and point `package.json`'s `win.icon` / `mac.icon`
  at those instead.
- **Code signing**: Windows and Mac will both show an "Unknown Publisher"
  warning to anyone who installs an unsigned app. Getting rid of that requires
  a paid code-signing certificate (Windows) and an Apple Developer account
  (Mac, $99/year) — not required to build, but worth planning for before a
  public release.
- **Auto-updates**: this basic setup does not include auto-update logic.
  `electron-builder` supports it, but it's an extra step wired up separately.
