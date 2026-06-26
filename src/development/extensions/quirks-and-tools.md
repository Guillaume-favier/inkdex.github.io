---
# SPDX-License-Identifier: GPL-3.0-or-later
# Copyright © 2026 Inkdex

prev:
  text: "Main.ts"
  link: "/development/extensions/main.ts"
next:
  text: "Tooling"
  link: "/development/tooling"
---

# Quirks and Tools

Before diving into building each capability, it's worth knowing how to debug your
extension while you work. Paperback ships with a few developer tools that will save
you a lot of time and guesswork.

## The Developer menu

Everything lives under **Settings → Developer**. You will find there the full list of log-files produced by your extension and by the app, sorted by time from newest to oldest. Each entry is tagged with its level, `DEBUG`, `WARN`, or `ERROR` so you can filter visually at a glance.

## Logging from your extension

Inside any method of your extension class, you have access to the standard JavaScript console:

```typescript
console.log("Fetched manga details for:", mangaId); // DEBUG
console.warn("No chapters found, returning empty list"); // WARN
console.error("Unexpected response shape:", response); // ERROR
```

All three levels show up in **Settings → Developer**. Use them freely while you develop, they cost nothing in production and make tracing a broken request much easier than adding and removing `throw` statements. But in production, do not abuse of them because having too much log ruins the quality of otherwise good code.

## Pulse, requests and live logs

Under **Settings → Developer → Open Pulse** you get two things at once:

- **Logs** -- the same entries as the developer list, but shown live as they come in.
- **Requests** -- every network request that went through your interceptor, including
  the URL, method, status code, and response time.

Pulse is particularly useful when you are working on the network layer of your extension. If a request is silently failing or returning an unexpected status, Pulse will show it immediately without you having to add extra logging. It's also good for a user to share quickly some error logs.

## Debug Server

You can go to **Settings → Developer → Debug Server** and enable it. This opens port `27015` of your device and streams all logs over gRPC in real time.

If you are on MacOS and you use the Mac version of Paperback, you can view the logs just by running :

```sh
npm run logcat
```

If Paperback is running on a different device on the same network (an iPhone or iPadrather than the Mac app), you can point `logcat` at it with the `--ip` flag:

```sh
npm run logcat -- --ip 192.168.1.42
```

You will see every log line printed to your terminal as it happens, colour-coded by
level:

- <span style="background:#00ff00;color:black;font-weight:bold;">[DEBUG]</span> for `console.log`
- <span style="background:#ffff00;color:black;font-weight:bold;">[WARN]</span> for `console.warn`
- <span style="background:#ff0000;color:white;font-weight:bold;">[ERROR]</span> for `console.error`

Each line also includes a Unix timestamp and any tags attached to the log entry.

The default port is `27015`, but you can override it with `--port` if needed.

## Throwing errors for fatal problems

Logging is for information you want to observe. Throwing is for situations where
your extension genuinely cannot continue and the user needs to know something went
wrong.

You already saw this pattern in `main.ts`:

```typescript
async getMangaDetails(mangaId: string): Promise<SourceManga> {
  // ...search through content...
  throw new Error("No title with this id exists");
}

async getChapters(sourceManga: SourceManga, sinceDate?: Date): Promise<Chapter[]> {
  // ...search through content...
  throw new Error("No title with this id exists");
}
```

When your extension throws, Paperback catches it and surfaces a visible error message
to the user instead of silently showing an empty screen. That distinction matters:
a missing manga ID is not a warning, it is a broken state. Throw for broken states,
log for everything else.

A rule of thumb:

- A method that **must** return something and cannot → `throw new Error(...)`
- Something unexpected happened but you can still return a result → `console.warn(...)`
- Something you just want to trace while developing → `console.log(...)`

---

With that toolkit in hand, you are ready to start building. The next sections go
through each capability one by one, starting from the foundation: `CHAPTER_PROVIDING`.
