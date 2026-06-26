---
# SPDX-License-Identifier: GPL-3.0-or-later
# Copyright © 2026 Inkdex

prev:
  text: "Your First Extension"
  link: "/development/extensions/start"
next:
  text: "Main.ts"
  link: "/development/extensions/main.ts"
---

# First Edits

In this guide we will look at the files common to all extensions, then edit them to make
this extension our own.

## How does an extension work?

Here is what the file tree of an extension looks like:

```txt
./static
./static/icon.png
./content.json
./forms.ts
./main.ts
./models.ts
./network.ts
./pbconfig.ts
```

When a user first downloads your extension, Paperback needs some basic information about
it. That is what `pbconfig.ts` is for.

### pbconfig.ts

This file exports a single object satisfying the `ExtensionInfo` type, which summarises
all the surface-level metadata of the extension.

#### Name

The `name` field is the display name of your extension. Spaces are allowed, but it is
cleaner to avoid them. The name should also match the directory where the code lives.
Do not use the raw domain as the name:

```
anilist.co   -->   AniList
```

#### Description

Technically you can write anything here, but the Inkdex convention is:

```
Extension that pulls content from anilist.co.
```

#### Version

The `version` string matters more than it might seem. Paperback allows updating between
any two extensions that share the same name, as long as their version strings differ,
even if the new version string is actually lower. This means you could accidentally
downgrade an extension if you are not careful.

A common scenario: you may have the same extension in multiple places (your local
machine, a stable GitHub release, etc.) with different version numbers. Be cautious when
prompted to update.

Note that you do not need to bump the version every time you want to test something on
your phone. Simply hit reload and it will reinstall.

At Inkdex, the first published version of an extension on an official repo starts at
`v1.0.0-alpha.1`.

#### Icon

The Paperback type definition explains this field well:

> An INTERNAL reference to an icon which is associated with this source.
> This Icon should ideally be a matching aspect ratio (a cube).
> The location of this should be in an includes directory next to your source.
>
> (`@paperback/type/lib/impl/SourceInfo.d.ts:86`)

In practice: create a `static/` directory inside your extension folder, place your icon
there, and set the `icon` field to just the filename (not the full path).

At Inkdex, we prefer square PNGs with a simple logo, not the full website name.

#### Language

Not strictly required, but trivial to add. Use lowercase
[ISO 639-1](https://www.loc.gov/standards/iso639-2/php/code_list.php) codes:

- `en` -- English-only content
- `es` -- Spanish-only content
- `multi` -- multiple languages

#### Content Rating

The Paperback type definition covers this clearly:

> A content rating attributed to each source. This can be one of three values, and
> should be set appropriately.
>
> - **Everyone:** This source does not have any sort of adult content available. Each
>   title within is assumed safe for all audiences.
> - **Mature:** This source MAY have mature content inside of it. Even if most content
>   is safe, Mature should be selected if even a small subset applies.
> - **Adult:** This source probably has straight up pornography available.
>
> This rating helps us filter your source to users who have the necessary visibility
> rules toggled for their profile. Naturally, only 'Everyone' sources will show up for
> users without an account, or without any mode toggles changed.
>
> (`@paperback/type/lib/impl/SourceInfo.d.ts:101`)

#### Capabilities

This is the most important field in the file. It is a list of `SourceIntents` enum
values and dictates exactly what the extension can and cannot do:

- `NONE` -- The extension does nothing.
- `CHAPTER_PROVIDING` -- The extension can read a chapter when asked to.
- `PROGRESS_PROVIDING` -- Can manage the user's reading progress to/from a website
  (often requires the user to be logged in).
- `DISCOVER_SECTION_PROVIDING` -- Can provide a homepage for the website in the Home
  section.
- `MANAGED_COLLECTION_PROVIDING` -- Lets you set up a Managed Collection that syncs to and from the website, where it can update reading lists, shelves, categories, etc. (also often requires login).
- `CLOUDFLARE_BYPASS_PROVIDING` -- The extension can use the built-in CloudFlare
  bypassing tools.
- `SETTINGS_FORM_PROVIDING` -- The extension exposes a custom settings page called the
  Settings Form.
- `SEARCH_RESULT_PROVIDING` -- The extension supports searching, either with simple
  sorting or advanced filters.

#### Badges

An array of `SourceBadge` objects. It can be empty and only affects how the extension
is displayed on the repository website.

Inkdex does not use badges, but some repositories use them to distinguish between
content types. For example, the
[Sinon Extension Repo](https://catta1997.github.io/Sinon-Paperback-Extensions/0.9/stable/)
uses a `Novel` tag with a dark green background, defined in their
[pbconfig.ts](https://github.com/Catta1997/Sinon-Paperback-Extensions/blob/0.9/stable/src/LNori/pbconfig.ts)
as:

```typescript
badges: [
  {
    label: "Novel",
    textColor: "#ffffff",
    backgroundColor: "#3baf4b",
  },
],
```

#### Developers

An array of `SourceDeveloper` objects. The list is shown both on the repository website
and inside the app, so only include what you are comfortable making public.

Only `name` is required. Both a `website` and a GitHub link are optional, but if you
provide both, only the `website` will be displayed. Multiple developers are supported.
