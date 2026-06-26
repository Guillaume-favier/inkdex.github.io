---
# SPDX-License-Identifier: GPL-3.0-or-later
# Copyright © 2026 Inkdex

prev:
  text: "First Edits"
  link: "/development/extensions/first-edits"
next:
  text: "Tooling"
  link: "/development/tooling"
---

# Main.ts

Now that `pbconfig.ts` is configured, we can look at `main.ts`, the heart of your
extension. This is the file that ties all of the other pieces together.

## Top comments

Just below the copyright header, you will find a TODO block. Think of it as a small
notebook: a convenient place to jot down what needs doing the next time you open the
file. You can also place TODO comments directly above any piece of code that still
needs work.

## Implementing the extension

Every extension exports a single class that implements `ExtensionImpl<T>`:

```typescript
export class ContentTemplateExtension implements ExtensionImpl<typeof ContentTemplateConfig>
```

The `<T>` generic, here `<typeof ContentTemplateConfig>` ties the class directly
to your `pbconfig.ts` configuration. This means the TypeScript compiler will enforce
that your class exposes exactly the methods that your declared capabilities require.
An extension that synchronises a reading library, for example, has no need to export
a function that reads a chapter.

This is also a good moment to rename all `ContentTemplate` references in the file.
Use your editor's search-and-replace to swap them for your extension's name, which
should match the name of its directory.

## Capabilities

As covered in [First Edits](/development/extensions/first-edits#capabilities), the
capabilities you declare in `pbconfig.ts` determine which methods your class must
implement. The table below is a quick reference, with full signatures for each
capability listed underneath.

### Quick reference

| Capability                     | Required | Optional |
| ------------------------------ | :------: | :------: |
| `NONE`                         |    0     |    0     |
| `CHAPTER_PROVIDING`            |    2     |    1     |
| `PROGRESS_PROVIDING`           |    3     |    0     |
| `DISCOVER_SECTION_PROVIDING`   |    2     |    0     |
| `MANAGED_COLLECTION_PROVIDING` |    3     |    0     |
| `CLOUDFLARE_BYPASS_PROVIDING`  |    0     |    2     |
| `SETTINGS_FORM_PROVIDING`      |    1     |    0     |
| `SEARCH_RESULT_PROVIDING`      |    1     |    2     |

### NONE

No methods required.

### CHAPTER_PROVIDING

```typescript
// Required
getChapters(sourceManga: SourceManga, sinceDate?: Date): Promise<Chapter[]>;
getChapterDetails(chapter: Chapter): Promise<ChapterDetails>;

// Optional
processTitlesForUpdates?(updateManager: UpdateManager, lastUpdateDate?: Date):  Promise<ChapterReadActionQueueProcessingResult>;
```

### PROGRESS_PROVIDING

```typescript
// Required
getMangaProgressManagementForm(sourceManga: SourceManga): Promise<Form>;
getMangaProgress(sourceManga: SourceManga): Promise<MangaProgress | undefined>;
processChapterReadActionQueue(actions: TrackedMangaChapterReadAction[]): Promise<ChapterReadActionQueueProcessingResult>;
```

### DISCOVER_SECTION_PROVIDING

```typescript
// Required
getDiscoverSections(): Promise<DiscoverSection[]>;
getDiscoverSectionItems(section: DiscoverSection, metadata?: Metadata): Promise<PagedResults<DiscoverSectionItem>>;
```

### MANAGED_COLLECTION_PROVIDING

```typescript
// Required
getManagedLibraryCollections(): Promise<ManagedCollection[]>;
commitManagedCollectionChanges(changeset: ManagedCollectionChangeset): Promise<void>;
getSourceMangaInManagedCollection(managedCollection: ManagedCollection): Promise<SourceManga[]>;
```

### CLOUDFLARE_BYPASS_PROVIDING

All methods for this capability are optional.

```typescript
// Optional
saveCloudflareBypassCookies?(cookies: Cookie[]): Promise<void>;
cloudflareBypassCompleted?(request: Request, cookies: Cookie[], localStorage: Record<string, string>): Promise<void>;
```

### SETTINGS_FORM_PROVIDING

```typescript
// Required
getSettingsForm(): Promise<Form>;
```

### SEARCH_RESULT_PROVIDING

```typescript
// Required
getSearchResults(
  query: SearchQuery<Metadata>,
  metadata: Metadata | undefined,
  sortingOption: SortingOption | undefined
): Promise<PagedResults<SearchResultItem>>;

// Optional
getSortingOptions?(query: SearchQuery<Metadata>): Promise<SortingOption[]>;
getAdvancedSearchForm?(query: SearchQuery<Metadata>): Promise<AdvancedSearchForm>;
```
