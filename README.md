# Image Rendering Test

This repo tests whether relative image paths render on GitHub in notebooks vs Markdown.

No Git LFS is used -- images are regular git objects.

## Markdown control test: relative path

![test image](images/test.png)

## Markdown control test: relative path with ./

![test image](./images/test.png)

## Markdown control test: HTML img tag

<img src="images/test.png" alt="test image" width="400"/>

If any of the above show a green rectangle, relative paths work in Markdown.

## Notebook test

See [test.ipynb](test.ipynb) for the notebook version of the same tests.
