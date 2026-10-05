---
duration: 10
---

# Styles, scripts and settings

Out of the box, every course gets a clean, accessible theme. This chapter shows how to make a course your own: brand colours and logo, your own CSS and JavaScript, extra HTML, a different page layout, and the interface language.

## Colours, logo and links

A few lines in `course.yml` cover most branding needs:

```yaml title="course.yml"
accent: "#0f8f7e"
accent_dark: "#5fd4c0"
logo: assets/logo.svg
favicon: assets/icon.png
home_url: https://learn.example.org/my-course
finish_url: https://example.org/feedback
edit_url: https://git.example.org/team/my-course/edit/main/{path}
```

`accent`
: The brand colour, used for links, buttons, the progress bar and highlights. Softer shades are derived from it automatically.

`accent_dark`
: The brand colour in dark mode. Optional: by default it's a lighter tint of `accent`, which usually works well.

`logo`
: An image shown before the course title in the top bar. Without a `favicon`, it's also used as the browser tab icon.

`favicon`
: The browser tab icon. Without it (and without a logo), the icon is the first letter of the course title on the accent colour.

`home_url`
: Adds a back arrow to the top bar that links to this address, for example the course's page on your learning platform.

`finish_url`
: Where the **Finish** button on the last chapter takes the reader, such as a feedback form or the next course. Without it, readers see a short congratulations message.

`edit_url`
: Adds an "Edit this chapter" link at the bottom of every chapter, handy for collecting fixes. `{path}` is replaced with the chapter's path inside the course (for example `chapters/01-basics/01-welcome.md`) and `{id}` with its id.

The logo, favicon and everything else are embedded in the HTML file like any image.

## Your own styles

List stylesheets under `styles:`. They're added to the page **after** the base theme, in the order you list them, so your rules win. Paths are relative to the course folder.

```yaml title="course.yml"
styles:
  - styles/custom.css
```

You can do two things in a stylesheet: change the **design tokens** of the base theme, and add **new classes** to use in your chapters.

### Design tokens

The base theme takes all its colours, fonts and sizes from CSS custom properties defined on `:root`. Change a token and every element that uses it follows. These are the most useful ones:

| Token | Default | Controls |
| --- | --- | --- |
| `--font-sans` | system font stack | All text |
| `--font-mono` | system monospace stack | Code |
| `--content-w` | `100%` | Maximum width of a chapter; the default fills the available area, a value such as `840px` gives a narrower reading column |
| `--sidebar-w` | `304px` | Width of the chapter list |
| `--rail-w` | `68px` | Width of the collapsed chapter list |
| `--topbar-h` | `56px` | Height of the top bar |
| `--radius`, `--radius-md`, `--radius-sm` | `4px`, `3px`, `2px` | Corner rounding of large, medium and small boxes (sharp by default) |
| `--radius-pill`, `--radius-dot` | `2px`, `2px` | Buttons, chips and badges; chapter and step numbers. Set `999px` and `50%` for pills and circles |
| `--bg`, `--surface`, `--surface-2`, `--surface-3` | light greys and white | Page background and box backgrounds |
| `--text`, `--text-2`, `--text-3` | near-black to grey | Main, secondary and muted text |
| `--border`, `--border-strong` | light greys | Lines and outlines |
| `--c-note`, `--c-tip`, `--c-warning`, `--c-danger`, `--c-exercise` | blue, green, amber, red, teal | Callout colours |
| `--code-bg`, `--code-hl` | near-white, accent tint | Code block background and highlighted lines |
| `--tk-keyword`, `--tk-string`, `--tk-comment` … | | Syntax highlighting colours |

Dark mode sets its own values for the colour tokens on `:root[data-theme="dark"]`. The reader's choice is stored on the page itself, so override dark colours with that selector rather than a `prefers-color-scheme` media query:

```css title="styles/custom.css"
/* Light mode (and anything that's the same in both modes) */
:root {
  --font-sans: "Atkinson Hyperlegible", system-ui, sans-serif;
  --content-w: 760px;
  --radius: 14px;
  --bg: #f6f3ee;
  --surface: #fffdf9;
}

/* Dark mode */
:root[data-theme="dark"] {
  --bg: #121212;
  --surface: #1b1b1d;
}
```

:::note
Set the brand colour with `accent` in `course.yml` rather than `--accent` in CSS: that way the dark-mode tint and the favicon use it too.
:::

### New classes

Add any class you like and use it from Markdown with an attribute or from raw HTML:

```css title="styles/custom.css"
.prose .lead {
  font-size: 1.15em;
  color: var(--text-2);
}
```

```markdown
{.lead}
This opening paragraph is a little larger than the rest.
```

Chapter content lives inside an element with the `prose` class, so prefixing your selectors with `.prose` keeps them away from the sidebar and top bar.

### Fonts and other files

`url(…)` references in your stylesheets are resolved relative to the stylesheet and **embedded**, just like images. That's how you ship a custom font that works offline:

```css title="styles/custom.css"
@font-face {
  font-family: "Atkinson Hyperlegible";
  src: url("../fonts/AtkinsonHyperlegible-Regular.woff2") format("woff2");
  font-weight: 400;
}
@font-face {
  font-family: "Atkinson Hyperlegible";
  src: url("../fonts/AtkinsonHyperlegible-Bold.woff2") format("woff2");
  font-weight: 700;
}
```

Use `.woff2` files: they're the smallest. A file that can't be found produces an `asset not found` warning. Addresses starting with `http://` or `https://` are left alone and loaded when the reader opens the page, so they don't work offline.

### One stylesheet for several courses

A path in `styles:` can point outside the course folder, so several courses can share a brand stylesheet kept in another folder of your working folder, for example a `brand/` repository next to `coursekit/`:

```yaml title="course.yml"
styles:
  - ../brand/brand.css
  - styles/custom.css
```

For a change that every course should get, edit the base theme itself in `coursekit/coursekit/theme/base.css`. Every course uses it on its next build.

:::tip
The live preview watches the course folder and coursekit's theme. After you edit a shared file somewhere else, save any chapter of the course to rebuild.
:::

### Starting from scratch

`replace_base_styles: true` leaves out the base theme entirely, so the page is styled only by your stylesheets. That's a lot of work: the sidebar, the transitions and every element need styles. Start from a copy of `coursekit/theme/base.css` if you go this way.

## Your own scripts

Files listed under `scripts:` are added after the built-in runtime, in order. A `.js` file is added as a regular script and an `.mjs` file as a JavaScript module.

```yaml title="course.yml"
scripts:
  - scripts/custom.js
```

Your scripts run after the runtime, so they can use the `window.coursekit` object and listen to its events right away.

| API | What it does |
| --- | --- |
| `coursekit.onChapter(fn)` | Calls `fn` with the current chapter right away, then again on every chapter change. |
| `coursekit.go(target)` | Opens a chapter by id (`'setup'`) or by position, counting from 0. |
| `coursekit.next()`, `coursekit.prev()` | The same as clicking **Next** or **Back**. On the last chapter, `next()` finishes the course. |
| `coursekit.toast(message)` | Shows a short message at the bottom of the screen for a few seconds. |
| `coursekit.current` | The position of the current chapter, counting from 0. |
| `coursekit.course` | The course data: `id`, `title`, `lang` and the list of `chapters`, each with `id`, `title`, `duration` and `part`. |

The runtime also dispatches two events on `document`:

`coursekit:chapterchange`
: Whenever a new chapter opens. `event.detail` holds the chapter's `index`, `id`, `title` and `element`, plus `previous`, the position of the chapter the reader came from. `onChapter` receives the same object.

`coursekit:finish`
: When the reader clicks **Finish** on the last chapter, just before they're sent to `finish_url` (or shown the congratulations message). Call `event.preventDefault()` in your listener to do something else instead, such as showing your own message.

```js title="scripts/custom.js"
// Cheer readers on as they approach the end.
coursekit.onChapter(({ index }) => {
  const left = coursekit.course.chapters.length - index - 1;
  if (left === 1) coursekit.toast('Almost there: one chapter to go!');
});

// Replace the default "course complete" message with your own.
document.addEventListener('coursekit:finish', (event) => {
  event.preventDefault();
  coursekit.toast(`Well done! You completed ${coursekit.course.title}.`);
});
```

## Extra HTML in the head

To add tags to the page's `<head>`, such as meta tags, a privacy-friendly analytics snippet or a verification tag, put them in an HTML file and point `head:` at it. Its contents are inserted as they are, after the styles.

```yaml title="course.yml"
head: partials/head.html
```

```html title="partials/head.html"
<meta name="author" content="Course Team">
<meta name="robots" content="noindex">
```

## Your own page layout

The page structure (top bar, sidebar, chapter area, Back and Next buttons) comes from `coursekit/theme/layout.html`. To change it for one course, copy that file into the course folder, edit the copy and point `layout:` at it:

```yaml title="course.yml"
layout: layout.html
```

The layout contains **placeholders**: a name between double curly braces, which the build replaces with generated content. For example:

```html
<title>{{ title }}</title>
<nav class="toc" aria-label="{{ label.chapters }}">{{ toc }}</nav>
<button class="pager-btn pager-prev" type="button" data-prev>{{ icon.chevron-left }}<span>{{ label.back }}</span></button>
```

There are three kinds of placeholder:

**Values** generated from your course:

| Name | Replaced with |
| --- | --- |
| `lang`, `title`, `description`, `version` | The language code, course title, subtitle (or description) and coursekit version |
| `favicon`, `styles`, `head` | The icon tag, all the styles, and your `head:` include |
| `home_link`, `brand_mark`, `course_title` | The back arrow (empty without `home_url`), the logo or letter mark, and the title |
| `first_chapter`, `chapter_count`, `time_total` | The first chapter's id, the number of chapters and the total "time remaining" text |
| `about`, `toc`, `chapters` | The About panel, the chapter list and all the chapters |
| `course_data`, `scripts` | The data the runtime reads, and all the scripts |

**Interface strings**: `label.` followed by a key, such as `label.next`. See [labels](#interface-language-and-labels) below.

**Icons**: `icon.` followed by an icon name: `panel`, `arrow-left`, `chevron-left`, `chevron-right`, `chevron-down`, `sun`, `moon`, `clock`, `check`, `copy`, `file`, `edit`, `reset`, `flag`, `info`, `bulb`, `check-circle`, `star`, `alert`, `octagon`, `pencil`, `question`, `layers` or `book`.

A misspelled placeholder or icon name produces a warning and is left empty.

:::warning Keep the hooks
The runtime finds its parts through ids and `data-` attributes, such as `id="sidebar"`, `data-prev`, `data-next`, `data-progress-bar` and `data-time-left`, and it reads the course data from the `<script type="application/json" id="course-data">` tag. Move them around as you like, but keep them, or navigation and progress stop working.
:::

## Interface language and labels

`language` picks the language of every built-in interface text: buttons, callout titles, the About panel and so on. coursekit includes English (`en`), Portuguese (`pt`) and Spanish (`es`). Regional codes fall back to their base language, so `pt-BR` uses the Portuguese texts. Any other code keeps the English texts but still sets the page language, and you can translate them with `labels`.

`labels` replaces individual texts. Some of the keys:

| Key | English text |
| --- | --- |
| `next`, `back`, `finish` | Next, Back, Finish |
| `remaining` | `{t} remaining` |
| `minutes`, `hours_minutes` | `{n} min`, `{h} h {m} min` |
| `part` | `Part {n}` |
| `about`, `reset_progress` | About this course, Reset progress |
| `course_complete` | You finished the course — nice work! |
| `quiz`, `quiz_check`, `quiz_correct` | Quick check, Check answer, Correct! |
| `note`, `tip`, `warning`, `exercise` … | Note, Tip, Warning, Exercise … (callout titles) |
| `details`, `hint`, `solution` | Details, Hint, Show solution |
| `copy`, `copied` | Copy, Copied |

The full list is in `coursekit/labels.py`. Keep the placeholders in curly braces, such as `{n}`, in your versions:

```yaml title="course.yml"
language: en
labels:
  next: Continue
  finish: Done!
  quiz: Check yourself
  tip: Pro tip
  remaining: "{t} to go"
  course_complete: All done. See you in the next course!
```

Values that start with `{` need quotes in YAML, as in `remaining` above.

:::quiz You'd like a darker page background in dark mode only. Where does the override of `--bg` belong?
- [ ] In `course.yml`, under `accent_dark`
- [x] In your stylesheet, under `:root[data-theme="dark"]`
- [ ] In your stylesheet, inside `@media (prefers-color-scheme: dark)`

The reader's light or dark choice is stored as `data-theme` on the page, so `:root[data-theme="dark"]` follows the switch in the top bar. `accent_dark` only sets the brand colour.
:::
