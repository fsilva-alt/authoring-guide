---
duration: 8
---

# How a course is organised

A course is a folder with a `course.yml` file and some Markdown chapters. In this chapter you'll see how coursekit finds your chapters, how you group them into parts, and how chapters link to each other.

![A course folder with course.yml and two parts of Markdown chapters is built into a single index.html that contains every chapter, the styles, the scripts, images and fonts](assets/structure.svg "One folder of Markdown in, one HTML file out")

## Folder layout

A course is any folder with a `course.yml`, and it lives next to the `coursekit/` folder in your working folder. Each course is its own git repository:

```text
work/
├── coursekit/                  ← the tool (its own repository)
└── my-course/                  ← a course (its own repository)
    ├── build.sh                ← builds the course with ../coursekit
    ├── course.yml              ← course settings
    ├── chapters/
    │   ├── 01-basics/          ← a part
    │   │   ├── _part.yml       ← the part's title
    │   │   ├── 01-welcome.md   ← a chapter (one Markdown file each)
    │   │   └── 02-setup.md
    │   └── 02-next-steps/
    │       ├── _part.yml
    │       └── 01-summary.md
    ├── assets/                 ← images, embedded at build time
    ├── styles/custom.css       ← optional extra styles
    ├── scripts/custom.js       ← optional extra behaviour
    ├── README.md
    └── dist/index.html         ← the build output (git-ignored)
```

Only `course.yml` and at least one chapter are required. The `assets/`, `styles/` and `scripts/` folder names are just a convention: you point to those files from `course.yml` or from your chapters.

## Course settings

`course.yml` holds the settings for one course. Only `title` really matters; everything else has a sensible default. Paths are relative to the course folder.

```yaml title="my-course/course.yml"
title: Introduction to Linux
subtitle: Find your way around the command line in one afternoon.
description: >
  A hands-on tour of the Linux shell for complete beginners.
authors:
  - Ada Lovelace
accent: "#0f8f7e"
numbering: part
styles:
  - styles/custom.css
```

### Every option

| Option | Default | What it does |
| --- | --- | --- |
| `title` | `Untitled course` | Course name in the top bar and the browser tab. |
| `subtitle` | empty | One-line summary, used for the page description (what search engines and link previews show). |
| `description` | empty | A paragraph (Markdown allowed) in **About this course**. |
| `authors` | none | A list of names, shown as "Written by". For a single name you can write `author: Ada Lovelace`. |
| `updated` | `auto` | The "Last updated" date. `auto` uses the newest chapter or `course.yml` file; you can also give a date such as `2026-10-04`, or `false` to hide it. |
| `language` | `en` | Interface language (`en`, `pt` or `es`), also used for the page language and date format. |
| `accent` | `#5b50e6` | Brand colour in light mode. |
| `accent_dark` | a lighter tint of `accent` | Brand colour in dark mode. |
| `logo` | none | Image shown before the title in the top bar; also the favicon unless you set one. |
| `favicon` | a letter on the accent colour | Icon in the browser tab. |
| `home_url` | none | Adds a back arrow to the top bar that links here, for example the course's page on your learning platform. |
| `finish_url` | none | Where **Finish** takes the reader on the last chapter. Without it, a "you finished" message appears. |
| `edit_url` | none | Adds an "Edit this chapter" link to each chapter. `{path}` is replaced with the chapter's path inside the course, `{id}` with its id. |
| `numbering` | `course` | Chapter numbers: `course`, `part` or `none` (see [numbering](#numbering)). |
| `transition` | `slide` | Animation between chapters: `slide`, `fade` or `none`. Readers who prefer reduced motion never see it. |
| `highlight` | `true` | Syntax highlighting for code blocks, done at build time. |
| `line_numbers` | `false` | Show line numbers on code blocks by default. |
| `embed_images` | `true` | Embed every image in the HTML file. With `false`, images stay as links and you must ship them next to the file. |
| `embed_external_images` | `true` | Also download and embed images from `http(s)` addresses. |
| `external_links_new_tab` | `true` | Open links to other websites in a new tab. |
| `typographer` | `true` | Turn straight quotes into curly ones and `--` into dashes. |
| `chapters_dir` | `chapters` | The folder that's scanned for parts and chapters. |
| `parts` | none | An explicit list of parts (see below). |
| `chapters` | none | A flat list of chapter files, for a course without parts. |
| `styles` | none | Stylesheets added after the base theme. |
| `replace_base_styles` | `false` | `true` drops the base theme, so only your stylesheets are used. |
| `scripts` | none | Scripts added after the built-in runtime. |
| `head` | none | An HTML file whose contents are added to the page's `<head>`. |
| `layout` | none | Your own copy of the page layout. |
| `labels` | none | Replacements for any interface text, such as the **Next** button. |
| `slug` | the folder name | The key under which the reader's browser stores their progress. Keep it stable once a course is published. |

The look-and-feel options (`styles`, `scripts`, `head`, `layout`, `language`, `labels`, colours and links) are covered in [Styles, scripts and settings](../03-customizing/01-styles-and-scripts.md).

:::warning
A misspelled option isn't an error: the build prints `unknown option '…' (ignored)` and carries on. Keep an eye on the build output, or build with `--strict` so warnings fail the build.
:::

## Parts

Parts group related chapters under a heading in the chapter list, such as "Part 1 · The basics" in this course. You can define them in three ways.

::::tabs
:::tab Folders
This is the default and the easiest. Each sub-folder of `chapters/` is a part, and its `_part.yml` gives it a title:

```yaml title="chapters/01-basics/_part.yml"
title: The basics
description: What this template does and how a course is organised.
```

`description` is optional; readers see it as a tooltip when they hover over the part in the chapter list or the part label above a chapter title. Without a `_part.yml`, the title comes from the folder name (`02-next-steps` becomes "Next steps").

Markdown files placed directly in `chapters/` come first, in a group without a title. A course made only of such loose files has no parts at all.
:::
:::tab List in course.yml
List the parts in `course.yml` when your files don't follow the folder layout. Each entry has a `title`, an optional `description`, and `chapters`, a list of paths or glob patterns:

```yaml title="course.yml"
parts:
  - title: Getting started
    chapters:
      - chapters/intro.md
      - chapters/setup/*.md
  - title: Going further
    chapters:
      - chapters/advanced/**/*.md
```

Glob matches are sorted by name. A path that doesn't exist, or a pattern that matches nothing, stops the build with an error. A part without a `title` becomes an untitled group.
:::
:::tab Flat list
For a short course without parts, list the chapters in order:

```yaml title="course.yml"
chapters:
  - chapters/welcome.md
  - chapters/setup.md
  - chapters/next-steps.md
```

Globs work here too, for example `chapters: [chapters/*.md]`. Each file is used once: if a file appears again (say, by name and again through a glob), the later entry is skipped, with a warning when you listed it by name.
:::
::::

Chapter files and part folders whose names start with `_` or `.` are skipped, so a `_notes.md` file or a `_drafts/` folder inside `chapters/` stays out of the build.

### Ordering

Chapters and parts are sorted by file and folder name. Start each name with a number so the order is obvious and stable:

```text
01-welcome.md
02-setup.md
…
10-wrap-up.md
```

Use leading zeros (`01`, `02` … `10`); otherwise `10-wrap-up.md` would sort before `2-setup.md`. The number prefix is dropped from chapter ids and from titles that coursekit derives from file names.

## Chapter files

A chapter is one Markdown file: optional front matter between `---` lines, then a single `# Title` heading, then your content organised with `##` and `###` headings.

```markdown title="chapters/01-basics/02-setup.md"
---
duration: 10
---

# Install the tools

Some introductory text.

## Check your version

…
```

### Front matter

All front matter keys are optional:

`title`
: The chapter title. Overrides the `# Title` heading; if neither exists, the title comes from the file name.

`id`
: The chapter's id, used in links and in the address bar. The default is the file name without its number prefix: `02-setup.md` becomes `setup`. Ids must be unique within a course.

`duration`
: Estimated reading time in whole minutes. It's shown in the chapter list and adds up to the course's "time remaining".

`draft`
: Set `draft: true` to leave the chapter out of the build while you work on it.

When you leave out `duration`, coursekit estimates it: roughly 200 words of prose per minute plus 15 lines of code per minute, rounded up, and never less than 1. The estimate is a good start, but an explicit value is better for hands-on chapters where readers spend time typing.

{#numbering}
### Numbering

`numbering` in `course.yml` controls the numbers in front of chapter titles:

| Value | Example | Notes |
| --- | --- | --- |
| `course` | 1, 2, 3, 4 … | Counts through the whole course. The default. |
| `part` | 1.1, 1.2, 2.1 … | Part number, then chapter number. This guide uses it. |
| `none` | | No numbers at all. |

## Chapter ids and links

Every chapter has an id, and every `##`, `###` and `####` heading gets an id generated from its text: lower case, punctuation removed, spaces turned into hyphens. The heading "Chapter ids and links" above gets `chapter-ids-and-links`. Ids are unique across the whole course; when two headings have the same text, the second one gets `-2` added, and so on.

You can link to them in several ways, all of which work in the built page:

| You write | Goes to |
| --- | --- |
| `[Elements](#elements)` | The chapter with id `elements` |
| `[Tabs](#tabs)` | The heading with id `tabs`, in whichever chapter it is |
| `[Elements](../02-writing/02-elements.md)` | That chapter: the file path is rewritten to `#elements` |
| `[Tabs](../02-writing/02-elements.md#tabs)` | That heading: rewritten to `#tabs` |

Links to `.md` files are resolved relative to the current chapter first and then from the course folder, so `chapters/02-writing/02-elements.md` works too. A link to a file that isn't a chapter of the course produces a warning. Try them: [the elements chapter](../02-writing/02-elements.md) and [its tabs section](../02-writing/02-elements.md#tabs).

### Custom heading ids

Generated ids change when you reword a heading, which breaks links to it. To give a heading a fixed id, put `{#your-id}` on the line just before it:

```markdown
{#numbering}
### Numbering
```

That's how the [numbering section](#numbering) above got its short id.

:::quiz A chapter file is named `03-first-steps.md` and has no `id` in its front matter. Which links open it from another chapter in the same folder?
- [x] `[First steps](#first-steps)`
- [ ] `[First steps](#03-first-steps)`
- [x] `[First steps](03-first-steps.md)`
- [ ] `[First steps](first-steps.html)`

The default id is the file name without its number prefix, `first-steps`. Links to `.md` files are rewritten to that id automatically.
:::

:::quiz Where do you set the title of a part when you use part folders?
- [ ] In the first chapter's front matter
- [x] In `_part.yml` inside the part's folder
- [ ] In the folder name, after the number prefix

Each part folder can hold a `_part.yml` with a `title` (and an optional `description`).
:::
