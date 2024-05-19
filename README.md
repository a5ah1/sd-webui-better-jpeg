# Better JPEGs for Stable Diffusion WebUI

This is a minimal, custom extension for the [Automatic1111 Stable Diffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) which I made for my own use. It sets some options I prefer for saving JPEG images.

This extension does not add any new options to set or alter the UI in any way. All it does is change one line in the `save_image_with_geninfo` function.

## What the added options do

1. `optimize=True` decreases file size without sacrificing image quality, but at the expense of an increase in processing time.
2. `progressive=True` for slightly better compression without quality loss.
3. `subsampling=0` turns off chroma subsampling for better preservation of fine color details. See [Chroma Subsampling in JPG Compression](https://matthews.sites.wfu.edu/misc/jpg_vs_gif/JpgCompTest/JpgChromaSub.html) for more information.

## Why does this exist?

For me, it makes sense for my workflow. I generate a lot of images, and the difference in visual quality between a PNG versus a JPEG image saved at ~93% quality *with the right options* is miniscule — but the size difference between the files can be quite significant.

## How it works

All this extension does is replace the `save_image_with_geninfo` function from `modules/images.py` with a version that is identical except it replaces this line:

```python
image.save(filename, format=image_format, quality=opts.jpeg_quality, lossless=opts.webp_lossless)
```

With this:

```python
image.save(filename, format=image_format, quality=opts.jpeg_quality, optimize=True, progressive=True, subsampling=0, lossless=opts.webp_lossless)
```

## Warnings

1. This extension works by overwriting the standard image-saving function. It is based on the function as it appears in [version 1.9.3](https://github.com/AUTOMATIC1111/stable-diffusion-webui/releases/tag/v1.9.3) of Auto1111. If your version is different, it's possible that this extension will break things.
2. This extension is not thoroughly tested at this point and comes with no guarantees whatsoever. Use at your own risk.