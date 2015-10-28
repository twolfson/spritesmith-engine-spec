# spritesmith-engine-spec

Specification for [spritesmith][] engines

In addition to this repo, we offer an integration test suite via [spritesmith-engine-test][].

[spritesmith]: https://github.com/Ensighten/spritesmith
[spritesmith-engine-test]: https://github.com/twolfson/spritesmith-engine-test

## Version
This documentation is for version:

**1.0.0**

## Documentation
### Terminology
In `spritesmith`, the following terms and definitions will be used:

- `engine` - A library that handles interactions between images
- `image` - Object containing metadata about an image file or buffer
- `canvas` - Class that handles placing images onto a visual layer and generating an output image

### Process
When `spritesmith` is generating a spritesheet, we will perform the following steps:

1. Process images into metadata (e.g. get size of each image)
2. Calculate layout based on user preferences (outside of `engine`)
3. Create canvas to place images on
4. Place each image on canvas
5. Export canvas as an image

### engine structure
A spritesmith `engine` returns the following properties on its `module.exports`:

- specVersion `String` - Current specification version the engine is supporting (e.g. '1.0.0')
- createCanvas `Function` - Utility to create `canvas` for `engine`
- createImages `Function` - Utility to create images which will later be laid out via a `canvas`

### `engine.createCanvas(width, height, cb)`
`engine.createCanvas` should have the function signature `(width, height, cb)`

- width `Number` - Width in pixels for the canvas
- height `Number` - Height in pixels for the canvas
- cb `Function` - Error-first callback function to return canvas via
    - `cb` will have the function signature `(err, canvas)`
    - If there is an error, run `cb(err)`. Otherwise, callback with a canvas (i.e. `cb(null, canvas)`)
    - This should be called asynchronously (e.g. if creation is synchronous, use `process.nextTick`)

### `engine.createImages(images, cb)`
`engine.createImages` should have the function signature `(images, cb)`

- images `String[]` - Array of filepaths to images
- cb `Function` - Error-first callback function to return image metadata via
    - `cb` will have the function signature `(err, images)`
    - If there is an error, run `cb(err)`. Otherwise, callback with an array of image metadata (i.e. `cb(null, images)`)
    - This should be called asynchronously (e.g. if creation is synchronous, use `process.nextTick`)
    - image `Object` - Metadata container about corresponding input image at same index
        - height `Number` - Height in pixels of corresponding input image at same index
        - width `Number` - Width in pixels of corresponding input image at same index
        - Any other metadata can be stored here and will be passed to `canvas.addImage` (e.g. `filepath`)

### canvas structure
A canvas for a spritesmith `engine` should be an object with the following structure:

- addImage `Function` - Method to add an image to canvas
- export `Function` - Method to export canvas as an image

### `canvas.addImage(image, x, y)`
`canvas.addImage` should have the function signature `(image, x, y)`

- image `Object` - Image object create from `engine.createImages`
    - This will be the **same** object as before so any additional metadata will be accessible (e.g. `filepath`)
- x `Number` - Horizontal coordinate to position left edge of image
- y `Number` - Vertical coordinate to position top edge of image

**Note:** This method is not asynchronous to force all asynchronous actions to take place during export. If there is an asynchronous action that needs to take place, then please store metadata on the `canvas` and use it during `export`.

### `canvas.export(options, cb)`
`canvas.export` should have the function signature `(options, cb)`

- options `Object` - Modifiers to indicate how to export (e.g. `format`, `quality`)
    - format `String` - Image format to export canvas as (e.g. `png`, `jpeg`)
    - Any other options can be defined custom to your engine (e.g. `quality`)
- cb `Function` - Error-first callback function to return export image via
    - `cb` will have the function signature `(err, result)`
    - If there is an error, run `cb(err)`. Otherwise, callback with an array of image metadata (i.e. `cb(null, result)`)
    - This should be called asynchronously (e.g. if creation is synchronous, use `process.nextTick`)
    - result `String` - Binary encoded string of output image (e.g. `Buffer.toString('binary')`)
        - This novice mistake was done from the inception of `spritesmith`. Thankfully it will be patched soon.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style.

## Donating
Support this project and [others by twolfson][gratipay] via [gratipay][].

[![Support via Gratipay][gratipay-badge]][gratipay]

[gratipay-badge]: https://cdn.rawgit.com/gratipay/gratipay-badge/2.x.x/dist/gratipay.png
[gratipay]: https://www.gratipay.com/twolfson/

## Unlicense
As of Oct 28 2015, Todd Wolfson has released this repository and its contents to the public domain.

It has been released under the [UNLICENSE][].

[UNLICENSE]: UNLICENSE
