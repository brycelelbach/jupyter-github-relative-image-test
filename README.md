# Notebook renderer does not display Git LFS images referenced via relative paths

Images stored in Git LFS and referenced via relative paths in Jupyter notebook markdown cells do not render on GitHub. The same images, referenced the same way from Markdown (`.md`) files in the same repository, render correctly.

## Reproduction

This repository contains two images in an `images/` directory:
- `images/test.png` -- a regular git object (not LFS)
- `images/lfs-test.png` -- tracked by Git LFS

Both are referenced identically from this README and [test.ipynb](test.ipynb) using `![alt](images/test.png)` syntax.

### Markdown: regular git object (green)

![green non-lfs](images/test.png)

### Markdown: Git LFS tracked (blue)

![blue lfs](images/lfs-test.png)

### Notebook

See [test.ipynb](test.ipynb) for the same two images rendered in a notebook.

### Results

| | Regular git object | Git LFS |
|---|---|---|
| **README.md** | Renders | Renders |
| **test.ipynb** | Renders | **Broken** |

## Root cause

GitHub's Markdown renderer resolves relative image paths through an endpoint that correctly handles LFS files (ultimately serving content via `media.githubusercontent.com`).

GitHub's notebook renderer appears to resolve relative image paths to `raw.githubusercontent.com`, which serves the LFS pointer file (`text/plain`, ~130 bytes) instead of the actual image content:

```
$ curl -sI "https://raw.githubusercontent.com/brycelelbach/jupyter-github-relative-image-test/main/images/lfs-test.png"
content-type: text/plain; charset=utf-8
```

The correct LFS content is available at `media.githubusercontent.com`:

```
$ curl -sI "https://media.githubusercontent.com/media/brycelelbach/jupyter-github-relative-image-test/main/images/lfs-test.png"
content-type: image/png
```

## Expected behavior

The notebook renderer should resolve relative image paths for LFS-tracked files the same way the Markdown renderer does, serving the actual image content rather than the LFS pointer.

## Workarounds

- Use absolute `https://media.githubusercontent.com/media/...` URLs in notebooks (loses relative path portability)
- Remove images from LFS tracking (increases repository size)
