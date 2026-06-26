---
# SPDX-License-Identifier: GPL-3.0-or-later
# Copyright © 2026 Inkdex

prev:
  text: "Development Guides"
  link: "/development/"
next:
  text: "Your First Extension"
  link: "/development/extensions/start"
---

# Extension Development

So you want to build an extension for Paperback - great, you're in the right place.
This guide will walk you through:

- Setting up an extension from scratch.
- Understanding what an extension needs to do, and what it is allowed to do.
- Using the right tools and debugging effectively.
- Organizing your extension's codebase.
- A deep dive into advanced features.

## Before you begin

### About Paperback 0.9

Throughout this guide, we will be building an extension for **Paperback 0.9** step by step.
Note that 0.9 is not yet available to everyone, but you can find out how to join the beta on the
[Inkdex Discord Server FAQ](https://discord.com/channels/965890377896845352/1361230107632599192).

### TypeScript prerequisite

All code in this guide is written in **TypeScript**, using **npm** as a package manager.
If you are not yet comfortable with TypeScript, the following resources are a good starting point:

- [The TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html) —
  the official reference, covering the language from the ground up.
- [JavaScript.info](https://javascript.info/) — if you are new to JavaScript entirely,
  start here. Make sure you are comfortable with `async`/`await` and Promises before
  moving on, as they are used extensively throughout.

### Web interaction prerequisite

Extensions retrieve data from websites using either a **REST API** or **HTML scraping**
(via Cheerio). Paperback provides custom helpers to handle CloudFlare protections for you,
so you do not need to deal with that directly, but you do need to understand the underlying
concepts:

- [RestAPITutorial.com](https://www.restapitutorial.com/) — a practical introduction to REST APIs.
- [Cheerio.js.org](https://cheerio.js.org/docs/intro/) — documentation for the HTML parsing library used in extensions.

> **Note:** This guide does not cover how to reverse-engineer a website
> (i.e. figuring out which endpoints return what data, or how to intercept requests).
> That knowledge is a prerequisite. If you need to build it up,
> [pptr.dev](https://pptr.dev/guides/what-is-puppeteer) and browser DevTools are good places to start.
