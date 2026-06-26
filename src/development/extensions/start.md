---
# SPDX-License-Identifier: GPL-3.0-or-later
# Copyright © 2026 Inkdex

prev:
  text: "Extension Development"
  link: "/development/extensions/"
next:
  text: "First Edits"
  link: "/development/extensions/first-edits"
---

# Your First Extension

This guide assumes you have **git** and **npm** installed. If not, you can get them here:
[git](https://git-scm.com/install/) · [npm / Node.js](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

Start by cloning the template repository and installing its dependencies:

```sh
git clone https://github.com/inkdex/template-extensions
cd template-extensions
npm install
```

This repository is the standard template for an Inkdex extension. It comes pre-configured
with all the tooling used across Inkdex's own extension repos.

## Repository structure

Here is a quick overview of what is inside:

- **`package.json`** : declares all dependencies and the scripts used to build, run, and debug the extension.
- **`tsconfig.json`** : TypeScript compiler settings shared across all Inkdex extensions.
- **`.oxfmtrc.json` / `.oxlintrc.json`** : formatting and linting rules to keep code style consistent.
- **`README.md`, `LICENSE`, `.gitignore`, `.github/`, `.husky/`** : standard project files; nothing to worry about for now.

All the code that matters lives in `src/`, which contains two directories:

- **`src/test/`** : test harness to verify that your extension runs correctly and has no major issues.
- **`src/ContentTemplate/`** : the extension itself. This is where you will spend most of your time.
  We will rename this directory in the next step to match your extension.

## Installing on your phone

Make sure your phone and computer are on the **same local network**, then run:

```sh
npm run dev
```

This command builds your extension and starts a local server, allowing you to load and
hot-reload the extension directly on your device. You should see output similar to:

```
Building Sources
Working directory: /home/user/template-extensions
✔ Bundle Sources [17ms]
  ✔ Transpiling Project [13ms]
  ✔ Generate SourceInfo [0ms]
    ✔ ContentTemplate [0ms]
✔ Generate Versioning File [3ms]
✔ Generate Homepage [2ms]
Server running at http://127.0.0.1:8080/
Server running at http://192.168.1.xx:8080/
For a list of commands do h or help
[23:24:33:0115]
```

The exact output may differ slightly, but the important line is the local network address
(`192.168.1.xx:8080`).

On your phone, add an extension repository using that address, for example
`http://192.168.1.105:8080/`. You should see **ContentTemplate** available to download.
That is your first extension, congratulations!

In the next guide, we will open up that extension and start editing it to make it our own.
