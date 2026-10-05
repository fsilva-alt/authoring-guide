---
duration: 5
---

# Welcome to coursekit

This guide is itself a course built with **coursekit**, the template you're about to use. The sidebar, the progress bar, the callouts and the quizzes you'll meet here all come from plain Markdown files and a single build command. Whenever you wonder how something was made, the chapter that explains it shows the source next to the result.

## What this template does

You write each chapter as a Markdown file. coursekit turns a whole course folder into **one self-contained `index.html`**: the chapters, the styles, the scripts, the images and even the fonts are embedded in that single file.

- **Nothing to install for your readers.** They open the file in a browser. It works from a web server, from a shared drive, or straight from disk.
- **No moving parts.** There's no database, no server-side code and no runtime downloads, so a course you build today still works years from now.
- **Plain text sources.** Chapters are Markdown, so you can review them, diff them and keep them in version control like any other code.

:::tip Learn by reading the source
This course is the `authoring-guide/` folder that sits next to `coursekit/`. Open its chapter files side by side with the built page whenever you want to see exactly how something was written.
:::

## A tour of the reader interface

Here's what your readers get, without any extra work from you.

::::columns
:::card Top bar
The **sidebar button** on the left shows or hides the chapter list. On the right you'll find the **time remaining** for the rest of the course and the **light/dark switch**. A thin **progress bar** along the bottom edge shows how far into the course you are.
:::
:::card Chapter list
Chapters are grouped into **parts**, each with a counter of finished chapters such as **1/2**. Every chapter shows its number and estimated minutes, and gets a **checkmark** once you've finished it. Click a part's heading to fold it.
:::
:::card Back and Next
Buttons at the bottom of each chapter move you through the course. **Next** shows the title of the chapter that follows and becomes **Finish** on the last one. You can also use <kbd>←</kbd> and <kbd>→</kbd>.
:::
:::card About this course
The panel at the top of the chapter list holds the course description, authors, last-updated date, total duration and a **Reset progress** button.
:::
::::

### On any screen size

The chapter list adapts to the width of the window:

| Screen | Chapter list | The sidebar button |
| --- | --- | --- |
| Wide (1100 px and up) | Full sidebar | Collapses it to a narrow rail of chapter numbers (remembered) |
| Medium (760 to 1099 px) | Narrow rail | Opens the full list on top of the page |
| Phone (below 760 px) | Hidden | Slides the list in as a drawer; tap outside it or press <kbd>Esc</kbd> to close |

### Progress is remembered

For each course, the browser remembers which chapters are done (a chapter counts as done when you click **Next** or **Finish**), the chapter you were reading and every ticked checklist item. It also remembers your preferred tabs, your light or dark choice and whether you collapsed the sidebar. When readers come back, they continue where they left off. Nothing leaves the reader's browser: it's all stored locally.

:::note
Every chapter and every heading has its own address. A link such as `index.html#welcome` opens this chapter directly, and hovering over a heading reveals a **#** link you can copy.
:::

## Quick start

coursekit lives in its own folder, and every course is a separate folder **next to it**, each in its own git repository:

```text
work/
├── coursekit/          ← the tool, shared by every course
├── python-basics/      ← an example course
├── authoring-guide/    ← this guide
└── intro-to-linux/     ← your next course
```

Each course has a small `build.sh` that runs `../coursekit`. Nothing from the tool is copied into the courses, so an improvement to coursekit reaches every course on its next build.

You need Python 3.9 or newer and a terminal: Linux, macOS, or WSL or Git Bash on Windows.

:::steps
1. **Put the tool and a course side by side** in one working folder:

   ```console
   $ git clone <coursekit repository> coursekit
   $ git clone <course repository> python-basics
   ```

2. **Preview** the course with live reload, then open <http://127.0.0.1:8000>. The first run also sets up coursekit's Python environment, which takes a minute. Leave the preview running while you write, and stop it with <kbd>Ctrl</kbd>+<kbd>C</kbd>.

   ```console
   $ cd python-basics
   $ ./build.sh dev
   ```

3. **Create** your own course. Run this from the working folder:

   ```console
   $ coursekit/bin/coursekit new intro-to-linux --title "Introduction to Linux"
   ```

4. **Build** it into a single file, `dist/index.html`:

   ```console
   $ cd intro-to-linux
   $ ./build.sh
   ```
:::

Your new course gets its own folder, `intro-to-linux/`, with a fresh git repository, a `build.sh` and a couple of sample chapters to edit.

[Next: how a course is organised](02-course-structure.md){.button}
