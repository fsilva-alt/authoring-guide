---
duration: 6
---

# Images

Images are embedded into the HTML file when you build, so a course never has broken images, even offline. This chapter covers where to put them, how to size and caption them, and how to make diagrams that look good in both light and dark mode.

## Adding an image

Use the usual Markdown syntax. The text in square brackets is the **alt text**: it describes the image for screen readers and appears if the image can't be shown, so make it meaningful.

```markdown
![A course folder is built into a single index.html](assets/structure.svg "One folder in, one file out"){width=520}
```

![A course folder is built into a single index.html](assets/structure.svg "One folder in, one file out"){width=520}

### Where coursekit looks for the file

| You write | coursekit looks in |
| --- | --- |
| `diagram.png` | The chapter's own folder first, then the course folder |
| `assets/diagram.png` | `assets/` next to the chapter first, then `assets/` in the course folder |
| `/assets/diagram.png` | The course folder only (a leading `/` means "from the course folder") |

Keeping all images in one `assets/` folder at the top of the course, as above, works from any chapter. If an image can't be found, the build prints `image not found` with the path, and the image is left as it is.

## Everything is embedded

Every `<img>` in a chapter is embedded, whether it comes from Markdown or from raw HTML. That means you can use HTML when you need more control. This one starts at the course folder, is centred, and has [zooming](#click-to-zoom) turned off:

```html
<p class="center"><img src="/assets/structure.svg" alt="The course structure diagram, small" width="300" data-zoom="false"></p>
```

<p class="center"><img src="/assets/structure.svg" alt="The course structure diagram, small" width="300" data-zoom="false"></p>

### Images from the web

Images with an `http://` or `https://` address are **downloaded when you build** and embedded too:

```markdown
![Project logo](https://example.org/images/logo.png)
```

Downloads are cached in `.cache/images/` inside the course folder (git-ignored), so later builds don't download them again and work offline. Delete that folder to fetch fresh copies.

If a download fails, for example because you're offline, the build prints a warning (`could not download …; keeping the external link`) and the image keeps pointing at its original address. It still shows up for readers who are online.

### Opting out

To leave an image out of the file, add `{data-embed=false}` after it in Markdown, or `data-embed="false"` in HTML. The image is then loaded from its address when the reader views the page, so make sure it's reachable from there.

```markdown
![A large photo](https://example.org/photos/large.jpg){data-embed=false}
```

Two settings in `course.yml` change this for the whole course: `embed_external_images: false` keeps every web image as a link, and `embed_images: false` turns off embedding altogether. With the latter you must ship your image files next to the HTML file yourself.

## Captions

Give an image a **title** (in quotes after the path) and it becomes a figure, with the title as its caption underneath. This happens when the image is alone in its paragraph. The caption can contain inline Markdown:

```markdown
![Chapter list on a phone](assets/phone.png "The chapter list slides in as a **drawer** on phones")
```

The diagram at the top of this chapter is captioned this way.

## Size

Images never grow wider than the text column, and they shrink on small screens. To make one smaller, set its width in pixels with an attribute:

```markdown
![Logo](assets/logo.svg){width=120}
```

You can also set `height`, or add a class such as `{.center}` on the line before the paragraph to centre an image that has no caption.

{#click-to-zoom}
## Click to zoom

Readers can click any image in a chapter to see it larger, and click again or press <kbd>Esc</kbd> to close it. Images inside links don't zoom, they follow the link. To turn zooming off for one image, add `{data-zoom=false}` (or `data-zoom="false"` in HTML), as in the small diagram above.

## Keep files small

Everything ends up in a single HTML file, and embedded images take about a third more space than the original files. The build warns you about any image over **2 MB** (`… is 2.4 MB — consider compressing it`).

- Use **SVG** for diagrams, charts and logos: small, and sharp at any size.
- Use **JPEG**, **WebP** or **AVIF** for photos and screenshots with lots of colours, and **PNG** for screenshots with large flat areas.
- Resize screenshots to about twice the width they're shown at; anything bigger is wasted.

## Diagrams for light and dark mode

Readers can switch between light and dark mode at any time, but an image looks the same in both. A diagram with dark lines on a transparent background is perfect in light mode and nearly invisible in dark mode.

:::tip Draw diagrams on their own background
Make the first shape of the diagram a filled rectangle with rounded corners, and choose colours that work on that background. The diagram at the top of this chapter uses a dark panel with light text and a teal accent, and reads well in both modes. Switch modes with the button in the top bar to check.
:::

A minimal SVG built that way:

```xml title="assets/badge.svg"
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 400 120">
  <rect width="400" height="120" rx="16" fill="#161821"/>
  <text x="200" y="68" text-anchor="middle" fill="#e8e9ef"
        font-family="system-ui, sans-serif" font-size="22">Readable in both modes</text>
</svg>
```

A few more tips for SVG files:

- **Always set a `viewBox`** so the image scales cleanly.
- **Keep it self-contained.** An SVG shown as an image can't load anything else: no web fonts, linked images or external stylesheets. Use system font stacks such as `system-ui, sans-serif`.
- **Leave room for text.** Fonts differ between systems, so give labels some spare space instead of fitting them exactly.

:::quiz You want one large photo to be loaded from your web server instead of being stored in the course file. What do you add after the Markdown image?
- [ ] `{embed=false}`
- [x] `{data-embed=false}`
- [ ] `{data-zoom=false}`
- [ ] Nothing: web images are never embedded

`data-embed=false` leaves that one image untouched. `data-zoom=false` only turns off click-to-zoom, and web images *are* embedded unless you opt out.
:::
