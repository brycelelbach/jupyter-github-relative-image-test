# Image Rendering Test: LFS vs Non-LFS

This repo tests whether Git LFS breaks image rendering in GitHub's notebook viewer.

- `images/test.png` -- green, **regular git object** (not LFS)
- `images/lfs-test.png` -- blue, **Git LFS tracked**

## Markdown control: regular git object (green)

![green non-lfs](images/test.png)

## Markdown control: Git LFS tracked (blue)

![blue lfs](images/lfs-test.png)

## Notebook test

See [test.ipynb](test.ipynb) for the same two images rendered in a notebook.

---

If you see both colors in the README but only green in the notebook, Git LFS is the problem.
