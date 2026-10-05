---
duration: 8
---

# Code blocks

Code blocks are fenced with three backticks, like in any Markdown. coursekit highlights them when you build, so no highlighting script runs in the reader's browser. Every block gets a header with its language and a **Copy** button.

## Options in the info string

The text right after the opening backticks is the **info string**. Its first word is the language; after that you can add options in any order:

````markdown
```python title="hello.py" hl_lines="2 4-6" linenums="10"
…
```
````

| Option | Example | What it does |
| --- | --- | --- |
| language | `python` | Picks the highlighter and the label in the header. |
| `title` | `title="app.py"` | Shows a title, usually a file name, in the header. |
| `hl_lines` | `hl_lines="2 4-6"` | Emphasises lines, counted from 1 within the block. Separate numbers and ranges with spaces or commas. |
| `{…}` | `{2,4-6}` | Short form of `hl_lines`. |
| `linenums` | `linenums` or `linenums="10"` | Shows line numbers, starting at 1 or at the number you give. |
| `nolinenums` | `nolinenums` | Hides line numbers when the course turns them on. |

### A title

````markdown
```python title="greet.py"
def greet(name):
    return f"Hello, {name}!"
```
````

```python title="greet.py"
def greet(name):
    return f"Hello, {name}!"
```

### Highlighted lines

````markdown
```python hl_lines="2 5-6"
def total_minutes(chapters):
    minutes = 0
    for chapter in chapters:
        print(chapter.title)
        minutes += chapter.duration
    return minutes
```
````

```python hl_lines="2 5-6"
def total_minutes(chapters):
    minutes = 0
    for chapter in chapters:
        print(chapter.title)
        minutes += chapter.duration
    return minutes
```

### Line numbers

Here the numbering starts at 10, to match the lines in the original file, and the short form highlights the third line of the block:

````markdown
```js {3} linenums="10"
document.addEventListener('coursekit:finish', () => {
  console.log('Course finished!');
  coursekit.toast('Well done!');
});
```
````

```js {3} linenums="10"
document.addEventListener('coursekit:finish', () => {
  console.log('Course finished!');
  coursekit.toast('Well done!');
});
```

:::note
Highlighted lines are always counted from 1 within the block, whatever number the line numbering starts at: the highlighted line above is shown as line 12, but you select it with `{3}`.
:::

### Settings for the whole course

Two options in `course.yml` set the defaults for every code block:

`highlight`
: `true` by default. Set it to `false` to show every code block as plain text.

`line_numbers`
: `false` by default. Set it to `true` to number every block with more than one line; add `nolinenums` to a block to switch them off there.

## Languages

You can use any language name or alias that the Pygments highlighter knows: several hundred of them. Here are some you're likely to need:

| For | Write |
| --- | --- |
| Shell scripts | `bash`, `sh`, `zsh` |
| Terminal sessions | `console` (or `shell-session`) |
| Windows | `powershell`, `ps1con` (sessions), `bat`, `doscon` (Command Prompt sessions) |
| Python | `python`, `pycon` (interactive sessions) |
| Data and config | `json`, `yaml`, `toml`, `ini`, `xml` |
| Web | `html`, `css`, `js`, `ts` |
| Data science and numerics | `r`, `julia`, `sql`, `fortran`, `matlab` |
| Systems | `c`, `cpp`, `rust`, `go`, `java` |
| Tooling | `dockerfile`, `makefile`, `diff` |
| No highlighting | `text` |

A language that isn't recognised is shown as plain text, and the build prints a warning so you can fix the typo.

A few examples:

::::tabs
:::tab SQL
```sql
SELECT title, duration
FROM chapters
WHERE duration > 5
ORDER BY duration DESC;
```
:::
:::tab YAML
```yaml
title: Introduction to Linux
numbering: part
labels:
  next: Continue
```
:::
:::tab C
```c
#include <stdio.h>

int main(void) {
    printf("Hello, world!\n");
    return 0;
}
```
:::
:::tab Fortran
```fortran
program hello
    implicit none
    print *, "Hello, world!"
end program hello
```
:::
:::tab Julia
```julia
function mean(xs)
    return sum(xs) / length(xs)
end
```
:::
:::tab R
```r
scores <- c(72, 88, 95)
mean(scores)
```
:::
:::tab Dockerfile
```dockerfile
FROM nginx:alpine
COPY dist/ /usr/share/nginx/html/
```
:::
:::tab Diff
```diff
-numbering: course
+numbering: part
 transition: slide
```
:::
::::

## Terminal sessions and REPLs

Use `console` for a terminal session: lines that start with a `$` prompt are commands, and the other lines are their output. Use `pycon` for the Python interactive prompt, with `>>>` and `...` prompts.

In both, the prompts can't be selected, and the **Copy** button copies **only the commands**, without prompts and without output. Readers can paste the result straight into their terminal.

````markdown
```console
$ ./build.sh
* authoring-guide
  -> dist/index.html  (363 KB, 8 chapters, 0.31s)
$ ls dist
index.html
```
````

```console
$ ./build.sh
* authoring-guide
  -> dist/index.html  (363 KB, 8 chapters, 0.31s)
$ ls dist
index.html
```

Press **Copy** above and paste somewhere: you get just the two commands.

```pycon
>>> chapters = ["welcome", "setup", "wrap-up"]
>>> for name in chapters:
...     print(name.upper())
...
WELCOME
SETUP
WRAP-UP
```

:::tip Long commands
To split a long command over several lines, end each line with `\`, as you would in the shell. The continuation lines belong to the command, so **Copy** includes them too.
:::

```console
$ COURSEKIT=/opt/coursekit ./build.sh \
    --strict --out public
* my-course
```

PowerShell (`ps1con`, with `PS>` prompts) and Command Prompt (`doscon`, with `C:\>` prompts) sessions work the same way.

## Plain text

Use `text` (or leave out the language) for program output, file trees, or anything that shouldn't be coloured. The header then shows only the Copy button.

```text
my-course/
├── build.sh
├── course.yml
└── dist/
    └── index.html
```

## Indented code blocks

Indenting lines by four spaces also makes a code block, as in classic Markdown. It's always plain text and takes no options, so prefer fences.

```markdown
A paragraph, then a blank line.

    This line is indented by four spaces.
```

A paragraph, then a blank line.

    This line is indented by four spaces.

## Code blocks that show code blocks

To show a fenced block inside a code block, as this guide does all the time, make the outer fence longer than the inner one. Four backticks around three works; for an example of that, add a fifth:

`````markdown
````markdown
```python
print("Hello")
```
````
`````

:::quiz You add the copy-friendly session below to a chapter. What lands on the clipboard when a reader clicks **Copy**?

```console
$ ./build.sh
* my-course
  -> dist/index.html
```
- [ ] All three lines
- [ ] `$ ./build.sh`
- [x] `./build.sh`
- [ ] Nothing: terminal sessions can't be copied

In `console` blocks the Copy button keeps only the commands and drops both the `$` prompt and the output.
:::
