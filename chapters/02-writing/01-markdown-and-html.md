---
duration: 8
---

# Markdown and HTML

Chapters are written in [CommonMark](https://commonmark.org), the standard flavour of Markdown, with a few popular extensions. Raw HTML is allowed anywhere, so you're never stuck when Markdown runs out of steam.

Each example below shows the Markdown first, followed by the result.

## The basics

Headings, paragraphs, **bold**, *italics*, `inline code`, [links](https://commonmark.org/help/), lists, quotes and horizontal rules all work the way you'd expect. If you're new to Markdown, the [ten-minute tutorial](https://commonmark.org/help/tutorial/) covers everything.

:::tip Headings inside a chapter
Start chapter sections at `##`. The single `#` heading at the top of the file is the chapter title, and coursekit lifts it into the chapter header.
:::

## Tables

Separate cells with `|` and put a row of dashes under the header. Colons in the dash row set the alignment.

```markdown
| Command | What it does        | Needs network |
| ------- | ------------------- | :-----------: |
| `build` | Writes `dist/`      |      No       |
| `dev`   | Preview with reload |      No       |
| `new`   | Copies the starter  |      No       |
```

| Command | What it does        | Needs network |
| ------- | ------------------- | :-----------: |
| `build` | Writes `dist/`      |      No       |
| `dev`   | Preview with reload |      No       |
| `new`   | Copies the starter  |      No       |

Wide tables scroll sideways on small screens instead of squeezing the text.

## Strikethrough and autolinks

```markdown
The build takes ~~minutes~~ seconds.

Web addresses become links on their own: https://commonmark.org
```

The build takes ~~minutes~~ seconds.

Web addresses become links on their own: https://commonmark.org

:::note Only full addresses become links
Only addresses that start with `http://` or `https://` are linked automatically. File names such as setup.py or notes.md stay plain text even though `.py` and `.md` are also internet domains, and so does a bare www.example.org. Write the full address, or use `[text](url)`, when you want a link.
:::

## Footnotes

```markdown
coursekit renders Markdown with markdown-it[^engine].

[^engine]: A fast, standards-compliant Markdown parser.
```

coursekit renders Markdown with markdown-it[^engine].

[^engine]: A fast, standards-compliant Markdown parser.

The footnote text appears at the end of the chapter, with a link back to where you were.

## Definition lists

Great for glossaries: a term on its own line, then one or more lines starting with `:`.

```markdown
Part
: A group of chapters with a shared heading in the chapter list.

Chapter
: One Markdown file. Readers see one chapter at a time.
```

Part
: A group of chapters with a shared heading in the chapter list.

Chapter
: One Markdown file. Readers see one chapter at a time.

## Smart typography

coursekit turns straight quotes into "curly" ones and replaces a few character sequences as you write:

| You type | You get |
| --- | --- |
| `"quotes"` and `'single'` | "quotes" and 'single' |
| `--` and `---` | -- and --- |
| `...` | ... |
| `(c)` `(tm)` `(r)` | (c) (tm) (r) |

Code is never changed. If you'd rather keep every character exactly as typed, set `typographer: false` in `course.yml`.

## Raw HTML

Any HTML tag works, inline or as a block. For an HTML block that contains Markdown, leave a **blank line** after the opening tag and before the closing tag. Without the blank lines, the content is passed through as-is and the Markdown isn't processed.

```markdown
<div class="center">

**Markdown works here**, thanks to the blank lines.

</div>

<div class="center">
**This stays as typed**, because there are no blank lines.
</div>
```

<div class="center">

**Markdown works here**, thanks to the blank lines.

</div>

<div class="center">
**This stays as typed**, because there are no blank lines.
</div>

## Attributes

Add classes, ids and other HTML attributes in curly braces: `.name` is a class, `#name` an id, and `key=value` any other attribute.

**After an inline element**, with no space in between. This works for links, images and inline code:

```markdown
Build with `--strict`{title="Fail the build on any warning"} in CI.
```

Build with `--strict`{title="Fail the build on any warning"} in CI. (Hover over the code to see its title.)

**On the line before a block** to style a whole paragraph, heading, list, table, quote, code block or `:::` element:

```markdown
{.center}
This paragraph is centred.
```

{.center}
This paragraph is centred.

On a code block or a `:::` element, the attributes land on its outer box, so a class from your own stylesheet can restyle that one box:

````markdown
{#setup-tip .wide}
:::tip
This callout has the id `setup-tip` and the extra class `wide`.
:::
````

## Built-in extras

The base theme includes a handful of small styles you can use straight away.

### Keys and highlights

```markdown
Press <kbd>Ctrl</kbd>+<kbd>C</kbd> to stop the server.
This is <mark>the important part</mark> of the sentence.
```

Press <kbd>Ctrl</kbd>+<kbd>C</kbd> to stop the server.
This is <mark>the important part</mark> of the sentence.

### Badges

```markdown
<span class="badge">New</span>
<span class="badge green">Stable</span>
<span class="badge amber">Beta</span>
<span class="badge red">Deprecated</span>
```

<span class="badge">New</span>
<span class="badge green">Stable</span>
<span class="badge amber">Beta</span>
<span class="badge red">Deprecated</span>

### Buttons

Add `{.button}` to a link, or `{.button .secondary}` for a quieter one. Combined with `{.center}` on the line before, you get a centred row of buttons:

```markdown
{.center}
[Start with the elements](02-elements.md){.button} [CommonMark help](https://commonmark.org/help/){.button .secondary}
```

{.center}
[Start with the elements](02-elements.md){.button} [CommonMark help](https://commonmark.org/help/){.button .secondary}

### Embeds

Wrap an `<iframe>` (a video player, a map, an interactive demo) in `<div class="embed">` to make it fill the width at a 16:9 ratio:

```html
<div class="embed"><iframe src="https://player.example.com/embed/12345" title="Intro video" allowfullscreen></iframe></div>
```

<div class="embed"><iframe title="An example embed" srcdoc="<body style='margin:0;height:100vh;display:grid;place-items:center;background:#0f8f7e;color:#fff;font:600 20px system-ui,sans-serif'>Your video, map or demo goes here</body>"></iframe></div>

:::warning
An embedded page is loaded from its own website when the reader opens the chapter. It's the one thing that isn't stored in your HTML file, so it won't show up offline.
:::
