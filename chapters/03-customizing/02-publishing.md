---
duration: 6
---

# Build and publish

When your chapters are ready, one command turns them into a file you can share. This chapter covers the build, the live preview, creating new courses, keeping coursekit up to date, and a checklist for publishing.

## Build a course

Every course has a `build.sh`. Run it from anywhere; it always builds its own course:

```console
$ cd my-course
$ ./build.sh
* my-course
  -> dist/index.html  (212 KB, 6 chapters, 0.14s)
```

The result is **one file**, `dist/index.html`, with every chapter, style, script and image inside. The `dist/` folder is git-ignored, so the sources stay the only thing you commit.

`build.sh` runs the coursekit that sits next to the course, `../coursekit`. If you keep coursekit somewhere else, point to it with the `COURSEKIT` variable:

```console
$ COURSEKIT=/opt/coursekit ./build.sh
```

## Share the file

Because a course is **a single HTML file** with everything inside, you can put it wherever your readers are:

::::columns
:::card Any web host
Upload the file to any static web host or web server. No server-side code or configuration is needed.
:::
:::card A learning platform
Most learning management systems accept an HTML file as a course resource or web page.
:::
:::card A shared drive
Put it on a network drive or in a shared folder. It opens from there, straight from disk, without a web server.
:::
:::card An attachment
Send it by email or chat. Readers download it and open it in their browser, even offline.
:::
::::

:::tip A way back to your platform
If the course lives inside a learning platform or a course website, set `home_url` in `course.yml` to that page. A back arrow then appears in the top bar.
:::

## Build options

Anything you add after `./build.sh` is passed to coursekit:

| Option | What it does |
| --- | --- |
| `--strict` | Exits with an error if there were any warnings. Use it in automated builds. |
| `--out public` | Writes `public/index.html` instead of `dist/index.html`. A path ending in `.html` sets the file name too. |
| `--help` | Lists every option. |

### Warnings and errors

The build prints a line starting with `!` for anything that deserves a look, and carries on:

```console
$ ./build.sh --strict
* my-course
  ! chapters/02-setup.md: link to unknown chapter 'instal.md'
  ! chapters/03-first-steps.md: image not found: assets/screen.png
  -> dist/index.html  (212 KB, 6 chapters, 0.14s)

2 warning(s).
```

Typical warnings are misspelled options in `course.yml`, missing images, links to chapters that don't exist, unknown code languages, very large images and images that couldn't be downloaded. Real errors, such as invalid YAML, a missing chapter file or two chapters with the same id, stop the build with an `error:` line.

:::important Use strict mode in automated builds
If you build a course automatically, for example on every change pushed to its repository, check out coursekit next to it and run `./build.sh --strict`. The build then fails on any warning, so a broken link or a missing image never reaches your readers.
:::

## Preview while you write

`./build.sh dev` builds the course, serves it at <http://127.0.0.1:8000> and **reloads the browser** whenever you save a file in the course, keeping your scroll position. It also rebuilds when coursekit's theme changes.

```console
$ ./build.sh dev
$ ./build.sh dev --port 9000 --open
```

Use `--port` to pick another port (to preview two courses at once, say) and `--open` to open the browser for you. Stop the preview with <kbd>Ctrl</kbd>+<kbd>C</kbd>.

:::danger Never publish the preview output
The preview writes its page to `.cache/dev/`. That page includes a script that keeps trying to reach the preview server, so it's not meant for readers. Always publish what `./build.sh` writes to `dist/`.
:::

## Start a new course

From your working folder, the one that contains `coursekit/`:

```console
$ coursekit/bin/coursekit new intro-to-linux --title "Introduction to Linux"
$ cd intro-to-linux
$ ./build.sh dev
```

`new` copies coursekit's starter template into a new folder `intro-to-linux/` and sets its title. It also creates a `build.sh` that points to `../coursekit`, a `.gitignore` and a short `README.md`, and runs `git init`; add `--no-git` to skip that last step. The starter's `course.yml` explains every option in comments, and its sample chapters, `styles/custom.css` and `scripts/custom.js` are ready for your own content.

To start from an existing course instead, add `--template` with its folder, such as `--template python-basics`.

## Keep coursekit up to date

Courses don't contain a copy of coursekit; they use it where it is. Improvements to the tool, the theme or the page layout therefore reach every course the next time it's built:

```console
$ cd coursekit && git pull && cd ..
$ cd my-course && ./build.sh
```

The first run after an update that changes coursekit's requirements refreshes its Python environment automatically.

## Before you publish

Run through this list before you share a course. Your ticks are remembered, so you can come back to it.

- [ ] `./build.sh --strict` prints no warnings.
- [ ] Every chapter has a realistic `duration`.
- [ ] No chapter you meant to publish still has `draft: true`.
- [ ] You've read the whole course once in light mode and once in dark mode.
- [ ] You've tried it in a narrow, phone-sized window.
- [ ] Every image has meaningful alt text, and diagrams are readable in both modes.
- [ ] Every quiz marks the right answers and explains them.
- [ ] `home_url`, `finish_url` and `edit_url` point to the right places.
- [ ] The title, authors and date in **About this course** are right.
- [ ] You're sharing `dist/index.html`, not the page in `.cache/dev/`.

## What next

That's the whole template. A good way to start:

1. Create a course with `new` and open it with `./build.sh dev`.
2. Write a welcome chapter, then one chapter per idea you want to teach.
3. Add exercises, hints and a quiz wherever readers should stop and practise.
4. Look at the `python-basics/` course for a complete example with custom styles and a script, and keep this guide open as your reference for the [elements](../02-writing/02-elements.md).

:::quiz Which of these are good ways to share a finished course?
- [x] Upload `dist/index.html` to a web host
- [x] Email `dist/index.html` to your readers
- [ ] Upload the `.cache/dev/` folder
- [ ] Send readers the course folder with your Markdown files

The built file in `dist/` contains everything. The preview output expects a running preview server, and the Markdown sources would have to be built first.
:::

:::success You're ready
You know how to organise a course, write chapters with every element, customise the look and publish the result. Click **Finish** to complete this guide, then go build your first course!
:::
