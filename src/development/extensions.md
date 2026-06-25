<!-- SPDX-License-Identifier: GPL-3.0-or-later -->
<!-- Copyright © 2025 Inkdex -->

# Extension Development

If you're reading this, it probably means you want to learn how to make an extension for Paperback. Awesome! You've come to the right place. This guide will teach you things such as:

- Making the base of an extension from scratch.
- Understanding what does an extension needs to do and what it can do.
- Using the right tool, and how to debug.
- How to organize the code of your extension.
- A detailed look on advanced features.
- And much more...

# Before you begin...

Together, we will be making step by step an extension for Paperback, but we will be making one for Paperback version 0.9. I understand that 0.9 isn't avaliable for everyone (for now) but I will share a secret with you if you make a modmail on the [official paperback server](https://discord.com/invite/paperback) saying that you are a developper, sharing you github account or other proof, they might grant you the role : `TestFlight Bypass` then you can access the Beta release of Paperback 0.9.
But if you don't code please don't bother asking them.
But if you already code a bit and think "Oh, I don't want to bother them..." please think again, they are glad to help you, Paperback wouldn't exist if people all around the world didn't contribute to this project, this is a great learning opportunity.

Also, we will be coding in TypeScript, using Npm as a packet manager. If you don't know how to code in TypeScript or you have only coded in JavaScript please follow those ressources

- [The TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [JavaScript.info](https://javascript.info/), if you don't know where to start because you know nothing about programming in JS, it's the place to be. Also, if you are not comfortable with async/await or Promises, review JavaScript fundamentals first.

We will also be getting some information form websites using either REST API or some scrapping (using Cheerio). Keep in mind that Paperback use some custom functions so that we don't have to deal with CloudFlare protections by ourselves, so you need to learn the key consepts but the execution will be detailed in specific guides

- [RestAPITutorial.com](https://www.restapitutorial.com/)
- [Cheerio.js.org](https://cheerio.js.org/docs/intro/)

To be clear once again, this guide will not show you how to "reverse engeneer" a website. If you don't know how to do it you can find a lot of ressources at [pptr.dev](https://pptr.dev/guides/what-is-puppeteer) we will not be covering this here, you need to know witch endpoint returns what etc...