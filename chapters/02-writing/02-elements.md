---
duration: 12
---

# Elements

coursekit adds a set of elements for teaching: callouts, exercises, collapsible hints and solutions, tabs, columns, cards, numbered steps, quizzes and checklists. Most of them are written as a block that starts with a line of three colons followed by the element's name, and ends with a line of three colons:

```markdown
:::name Optional title
Any Markdown here: paragraphs, lists, code blocks, images…
:::
```

Every example in this chapter shows the Markdown first, followed by the live result.

## Callouts

Callouts make a paragraph stand out. There are eight kinds, each with its own colour and icon:

```markdown
:::note
Background information that's useful, but not essential.
:::

:::info
Same look as a note. Use whichever word reads better.
:::

:::tip
A shortcut or a better way of doing something.
:::

:::success
Confirms that the reader got something right.
:::

:::important
Something readers shouldn't skip.
:::

:::warning
Something that could go wrong if readers aren't careful.
:::

:::caution
A mistake that's easy to make and hard to undo.
:::

:::danger
Same look as caution: for anything that can lose data or break things.
:::
```

:::note
Background information that's useful, but not essential.
:::

:::info
Same look as a note. Use whichever word reads better.
:::

:::tip
A shortcut or a better way of doing something.
:::

:::success
Confirms that the reader got something right.
:::

:::important
Something readers shouldn't skip.
:::

:::warning
Something that could go wrong if readers aren't careful.
:::

:::caution
A mistake that's easy to make and hard to undo.
:::

:::danger
Same look as caution: for anything that can lose data or break things.
:::

### Custom titles

Without a title, a callout is labelled with its kind ("Note", "Tip" …, in the course's language). Write your own title after the name; it can contain inline Markdown:

```markdown
:::warning Stop the preview first
Press <kbd>Ctrl</kbd>+<kbd>C</kbd> in its terminal before you rename the course folder.
:::

:::success You built your first course!
Open `dist/index.html` in your browser to see it.
:::
```

:::warning Stop the preview first
Press <kbd>Ctrl</kbd>+<kbd>C</kbd> in its terminal before you rename the course folder.
:::

:::success You built your first course!
Open `dist/index.html` in your browser to see it.
:::

### GitHub-style callouts

You can also write callouts as block quotes that start with `[!KIND]`, the syntax many code hosts understand. Any of the eight kinds works, and an optional title can follow on the same line:

```markdown
> [!TIP]
> This syntax also looks reasonable in plain Markdown previewers.

> [!IMPORTANT] Read this first
> The title goes after the kind, on the same line.
```

> [!TIP]
> This syntax also looks reasonable in plain Markdown previewers.

> [!IMPORTANT] Read this first
> The title goes after the kind, on the same line.

Because they don't use colons, these callouts are handy inside tabs and other elements, where you'd otherwise need to [count colons](#nesting-elements).

## Exercises

An exercise box frames a hands-on task. Follow it with a hint and a solution (see the next section) so readers can check their work:

```markdown
:::exercise Plan your first course
Write down the title of a course you'd like to create and three chapters it could have.
:::

:::hint
Think of something you've explained to colleagues more than once.
:::

:::solution
There's no single right answer. For example: *Introduction to Linux*, with chapters on
the shell, on files and folders, and on permissions.
:::
```

:::exercise Plan your first course
Write down the title of a course you'd like to create and three chapters it could have.
:::

:::hint
Think of something you've explained to colleagues more than once.
:::

:::solution
There's no single right answer. For example: *Introduction to Linux*, with chapters on
the shell, on files and folders, and on permissions.
:::

## Collapsible sections

Three elements start closed and open when the reader clicks them. Each takes an optional summary text:

| Element | Default summary | Use it for |
| --- | --- | --- |
| `:::details` | "Details" | Extra background, long output, edge cases |
| `:::hint` | "Hint" (with a light bulb) | A nudge in the right direction |
| `:::solution` | "Show solution" (with a check mark) | The answer to an exercise |

```markdown
:::details What does "self-contained" mean?
Everything the page needs is stored inside the HTML file itself: no extra files, no downloads.
:::

:::hint Stuck? Open this
Hints work just like details, with a different icon.
:::
```

:::details What does "self-contained" mean?
Everything the page needs is stored inside the HTML file itself: no extra files, no downloads.
:::

:::hint Stuck? Open this
Hints work just like details, with a different icon.
:::

When a reader prints a chapter, every collapsed section is opened so nothing is missing on paper.

## Tabs

Tabs show alternatives side by side, such as instructions for different operating systems. Wrap a `:::tab Label` block for each tab inside a `::::tabs` block. Notice that `tabs` has **four** colons: it contains the three-colon tabs (more about this in [Nesting elements](#nesting-elements)).

```markdown
::::tabs
:::tab Windows
Press <kbd>Win</kbd>, type **Terminal** and press <kbd>Enter</kbd>.
:::
:::tab macOS
Press <kbd>Cmd</kbd>+<kbd>Space</kbd>, type **Terminal** and press <kbd>Return</kbd>.
:::
:::tab Linux
Press <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd>.
:::
::::
```

::::tabs
:::tab Windows
Press <kbd>Win</kbd>, type **Terminal** and press <kbd>Enter</kbd>.
:::
:::tab macOS
Press <kbd>Cmd</kbd>+<kbd>Space</kbd>, type **Terminal** and press <kbd>Return</kbd>.
:::
:::tab Linux
Press <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd>.
:::
::::

Tab groups are **synced by label**: choosing a tab switches every group on the page that has a tab with exactly the same label. The choice is also **remembered**, so readers who pick Linux once see Linux tabs in every chapter and on their next visit. Try it: pick a tab above and this group follows.

::::tabs
:::tab Windows
```powershell
py --version
```
:::
:::tab macOS
```console
$ python3 --version
```
:::
:::tab Linux
```console
$ python3 --version
```
:::
::::

:::tip
Keep labels consistent across a course (`macOS` everywhere, not `Mac` in one place), because only identical labels are synced. Readers can also move between tabs with the arrow keys.
:::

## Columns and cards

### Columns

Put content side by side with `:::column` blocks inside a `::::columns` block. Columns sit next to each other when there's room (at least about 250 pixels each) and stack on narrow screens.

````markdown
::::columns
:::column
**Before**

```python
print "Hello"
```
:::
:::column
**After**

```python
print("Hello")
```
:::
::::
````

::::columns
:::column
**Before**

```python
print "Hello"
```
:::
:::column
**After**

```python
print("Hello")
```
:::
::::

### Cards

A card is a quiet box with an optional title, for side content that isn't a warning or a tip:

```markdown
:::card Did you know?
This whole guide is a single HTML file, like every course you'll build.
:::
```

:::card Did you know?
This whole guide is a single HTML file, like every course you'll build.
:::

### Card grid

Cards placed directly inside `::::columns` (without `:::column`) form a grid of equal-height cards:

```markdown
::::columns
:::card Write
Chapters are plain Markdown files.
:::
:::card Build
One command produces one HTML file.
:::
:::card Share
Host it anywhere, or simply send it.
:::
::::
```

::::columns
:::card Write
Chapters are plain Markdown files.
:::
:::card Build
One command produces one HTML file.
:::
:::card Share
Host it anywhere, or simply send it.
:::
::::

## Steps

Wrap a numbered list in `:::steps` to turn it into a procedure with large step numbers joined by a line. To put a code block or a second paragraph inside a step, indent it to line up with the step's text (three spaces):

````markdown
:::steps
1. Create a folder for your course next to `coursekit/`.
2. Add a `course.yml` file with a title:

   ```yaml
   title: My first course
   ```

3. Write your first chapter in `chapters/01-welcome.md`.
:::
````

:::steps
1. Create a folder for your course next to `coursekit/`.
2. Add a `course.yml` file with a title:

   ```yaml
   title: My first course
   ```

3. Write your first chapter in `chapters/01-welcome.md`.
:::

## Quizzes

A quiz is a quick knowledge check. Write the question after `:::quiz`, then the answers as a task list: `[x]` marks a correct answer and `[ ]` a wrong one. Anything after the list is the **explanation**, shown once the question is solved.

### Single choice

With exactly one correct answer, readers click an answer. A wrong answer is marked and they can try again, or click **Show answer**.

```markdown
:::quiz Which command writes `dist/index.html`?
- [ ] `./build.sh dev`
- [x] `./build.sh`
- [ ] `coursekit/bin/coursekit new my-course`

`./build.sh dev` starts the live preview, and `new` creates a course from the starter template.
:::
```

:::quiz Which command writes `dist/index.html`?
- [ ] `./build.sh dev`
- [x] `./build.sh`
- [ ] `coursekit/bin/coursekit new my-course`

`./build.sh dev` starts the live preview, and `new` creates a course from the starter template.
:::

### Multiple choice

Mark more than one answer with `[x]` and the quiz becomes multiple choice: readers see "Select all that apply", tick their answers and click **Check answer**.

```markdown
:::quiz Which of these are stored inside the built HTML file?
- [x] Your chapters
- [x] Images from the `assets/` folder
- [x] Your custom stylesheets
- [ ] The dev server

Everything a reader needs is embedded. The dev server is only a tool for you, the author.
:::
```

:::quiz Which of these are stored inside the built HTML file?
- [x] Your chapters
- [x] Images from the `assets/` folder
- [x] Your custom stylesheets
- [ ] The dev server

Everything a reader needs is embedded. The dev server is only a tool for you, the author.
:::

Quizzes are for practice: answers aren't graded, sent or saved.

## Checklists

A task list outside a quiz becomes a checklist that readers can tick off. Their ticks are **remembered** in the browser, until they click **Reset progress** in the About panel. Items written with `[x]` start ticked.

```markdown
- [x] Pick a course title
- [ ] Write the welcome chapter
- [ ] Add a quiz to every part
```

- [x] Pick a course title
- [ ] Write the welcome chapter
- [ ] Add a quiz to every part

## Nesting elements

Elements can contain other elements. The rule is simple: **the outer element needs more colons than the elements inside it**. An element ends at the first line that has at least as many colons as its opening line and nothing else. If the outer element also opened with three colons, the closing line of the inner element would end it too early.

````markdown
::::tip Nesting in action
This tip contains a collapsible section.

:::details Why four colons?
The tip opens with four colons, so the three-colon lines inside can't close it.
:::
::::
````

::::tip Nesting in action
This tip contains a collapsible section.

:::details Why four colons?
The tip opens with four colons, so the three-colon lines inside can't close it.
:::
::::

Each extra level adds a colon. Here a callout sits inside a column, inside a columns block:

```markdown
:::::columns
::::column
:::note
On the left.
:::
::::
::::column
:::tip
On the right.
:::
::::
:::::
```

:::::columns
::::column
:::note
On the left.
:::
::::
::::column
:::tip
On the right.
:::
::::
:::::

:::important Code blocks count too
coursekit finds the closing line of an element before it reads the code blocks inside it. If an element contains a code block with three-colon lines in it (like the examples in this guide), give the element more colons than those lines, as in the exercise below.
:::

:::exercise Write a callout
Write a warning callout titled "Save your work" that reminds readers to save the file before they close their editor.
:::

:::hint
The title goes on the opening line, right after the kind: `:::warning Your title`.
:::

::::solution
````markdown
:::warning Save your work
Save the file before you close your editor.
:::
````

This solution box opens with four colons, because the code block inside it contains three-colon lines.
::::

## Cheat sheet

| Element | Syntax |
| --- | --- |
| Callout | `:::note`, `info`, `tip`, `success`, `important`, `warning`, `caution`, `danger`, each with an optional title |
| Callout, quote style | `> [!TIP]` (any kind), with an optional title on the same line |
| Exercise | `:::exercise Title` |
| Collapsible | `:::details Summary`, `:::hint`, `:::solution` |
| Tabs | `::::tabs` around `:::tab Label` blocks |
| Columns | `::::columns` around `:::column` blocks |
| Card | `:::card Title`; directly inside `::::columns` for a grid |
| Steps | `:::steps` around a numbered list |
| Quiz | `:::quiz Question` with `- [x]` right and `- [ ]` wrong answers, then the explanation |
| Checklist | `- [ ]` items outside a quiz |
