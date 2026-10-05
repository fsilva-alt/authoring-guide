# Authoring Guide

The documentation for writing courses with **coursekit**, written as a course itself:
how courses are organised into parts and chapters, every element you can use in a chapter
(each shown as Markdown source next to the live result), code blocks, images, custom styles
and scripts, and publishing.

## Build

Needs the coursekit repository next to this one (`../coursekit`) and Python 3.9+.

```bash
./build.sh            # build dist/index.html: open it in a browser
./build.sh dev        # preview at http://127.0.0.1:8000, rebuilt and reloaded as you save
```
