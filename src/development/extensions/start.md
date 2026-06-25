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

# Your first Extension

If you are here, I hope you can use git and npm, (if not, download it [here](https://git-scm.com/install/) and [here](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm))

then you can git clone this repo and install the tooling with

```sh
git clone https://github.com/inkdex/template-extensions
cd template-extensions
npm install
```

This repo is a template for a Paperback extension with all of the classic tools found inside the main repos of Inkdex extensions. You will find in it

- a `package.json` file with all the tools to install and all the scripts to run and debug.
- a `tsconfig.json` file with all of our typescript settings.
- a `.oxfmtrc.json` and `.oxlintrc.json` to ensure that the code has the same formatting and style.
- Other files and directories that do not interest us here, like `README.md`, `LICENSE`, `.gitignore`, `.github/`, `.husky/` ...
- but all the interesting files are in `src/`. In there, you will see two dirs, a `src/test` and `ContentTemplate` directory.
  - the `src/test` directory contains all the code to test that our extension(s) are running correctly and that there are no major issues
  - the `src/ContentTemplate` is where we will spend most of our time, it's the home of the extension but we will soon change it's name

Now that we know the general organization of an Inkdex extension repository, we will try to install the template extension on our phone, make sure that you are connected on the same network then on your computer, run the command :

```sh
npm run dev
```

This command let's us test our extension and update the extension on our phone quickly.

It should print something like :

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
Server running at http://192.168.1.105:8080/

For a list of commands do h or help
[23:24:33:0115]
```

It probably wont be exacly like this but it's very usefull.
On your phone, try to add an extension repository with your local ip like `http://192.168.1.105:8080/`, you will see that you can now download "ContentTemplate", it's your first extension, congratulation!

In the next guide we will see what's in an extension and we will edit it to make it ours
