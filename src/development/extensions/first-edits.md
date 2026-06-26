---
# SPDX-License-Identifier: GPL-3.0-or-later
# Copyright © 2026 Inkdex

prev:
  text: "Your First Extension"
  link: "/development/extensions/start"
next:
  text: "Tooling"
  link: "/development/tooling"
---

# First Edits

Here, we will see the common files that are in all of the extensions and we will edit them to make this extension ours!

## How does an extension work?

First let's look at what files are in this extension :

```
./static
./static/icon.png
./content.json
./forms.ts
./main.ts
./models.ts
./network.ts
./pbconfig.ts
```

When you first download the extension, you need to provide to Paperback all of the informations, and this is what the `pbconfig.ts` file is for.

### pbconfig.ts

We can see that this files export an object (that satisfies the type `ExtensionInfo`) that resumne all of the surface info of the extension

#### Name

You can now edit the `name` of the app, it can contain spaces but it's betten if there are none and if the app name is the same then the name of the directory where the code lives. Also, do not put the whole url as the name, for exemple : `mangakatana.com` --> `MangaKatana`

#### Description

For the `description` part, you could also technically put anything in there but the common syntax for Inkdex is : `Extension that pulls content from mangakatana.com.`

#### Version

The `version` string is very important, you should know that Paperback alows you to "update" from any 2 extensions with the same name as long as the version string is different even when it's a lower version. It's important because you might have different extension repositories with differrent version of you extension (the latest one on your PC, the stable one on GitHub etc...) so be carefull and do not update everytime you're prompted to. When you will want to debug something on your phone, you don't have to bump the version everytime, you can just hit reload it reinstalls it.

#### Icon

For the `icon` attributes, I will let the Paperback comment explain this one :

> An INTERNAL reference to an icon which is associated with this source.
> This Icon should ideally be a matching aspect ratio (a cube)
> The location of this should be in an includes directory next to your source.
>
> ... (`@paperback/type/lib/impl/SourceInfo.d.ts:86`)

This just means, in the directory of your extension, you should have a `static` dir, with an icon, and you should put only the filename of this icon inside the `icon` attributes.

At Inkdex we preffer square png with only a simple logo and not the fullname of the website

#### Language

It's not technically required, but it cost nothing to add it. If your website only provides english content : `en`, if there are multiple langages : `multi`, and if there is only spanish content `es`. Please follow the lowercase version of [ISO 3166-1 alpha-2](https://wikipedia.org/wiki/ISO_3166-1_alpha-2)

#### Content Rating

> A content rating attributed to each source. This can be one of three values, and should be set appropriately.
>
> Everyone: This source does not have any sort of adult content available. Each title within is assumed safe for all audiences
>
> Mature: This source MAY have mature content inside of it. Even if most content is safe, mature should be selected even if a small subset applies
>
> Adult: This source probably has straight up pornography available.
>
> This rating helps us filter your source to users who have the necessary visibility rules toggled for their profile.
> Naturally, only 'Everyone' sources will show up for users without an account, or without any mode toggles changed.
>
> (`@paperback/type/lib/impl/SourceInfo.d.ts:101`)

#### Capabilities

This is the most important part of this file, it dictates everything that the extension can and cannot do, it's a list of `enums SourceIntents`, possibilities are the following :

- `NONE` - the extension does nothing.
- `CHAPTER_PROVIDING` - You can read a chapter when asked to.
- `PROGRESS_PROVIDING` - ??
- `DISCOVER_SECTION_PROVIDING` - Can provide a homepage for your website in the Home section.
- `MANAGED_COLLECTION_PROVIDING` - ??
- `CLOUDFLARE_BYPASS_PROVIDING` - Your extension can use the CloudFlare bypassing tools
- `SETTINGS_FORM_PROVIDING` - Your extension provides a special settings page called the Settings form.
- `SEARCH_RESULT_PROVIDING` - Your can search stuff with your extension. Either a simple sort or even with advanced filters.
