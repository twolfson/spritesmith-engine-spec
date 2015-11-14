# spritesmith-engine-spec

Specification for [spritesmith][] engines

In addition to this repo, we offer an integration test suite via [spritesmith-engine-test][].

[spritesmith]: https://github.com/Ensighten/spritesmith
[spritesmith-engine-test]: https://github.com/twolfson/spritesmith-engine-test

## Version
This documentation is for version:

**2.0.0**

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

### Exports
A spritesmith engine returns an `Engine` constructor function with as its `module.exports`

### `new Engine(options)`
Constructor for a new engine

- options `Object` - Container for flags to set for the engine
    - For example, `gmsmith` has `imagemagick: true` to force usage of ImageMagick over GraphicsMagick

### `Engine.specVersion`
`String` representing current specification version the engine is supporting (e.g. '1.0.0')

### `engine.createCanvas(width, height)`
`Function` to create a new canvas based on the `engine`. This should have the function signature `(width, height, cb)`

- width `Number` - Width in pixels for the canvas
- height `Number` - Height in pixels for the canvas

**Returns:**

- canvas `Canvas` - Instance of a canvas class for the `engine`

**Note:** This method is intentionally synchronous. Please run all asynchronous actions during `canvas.export`. We suggest saving any critical metadata to `canvas` and reusing it during `canvas.export`.

### `engine.createImages(images, cb)`
`Function` to create images which will later be laid out via a `canvas`. This should have the function signature `(images, cb)`

- images `String[]` - Array of filepaths to images
- cb `Function` - Error-first callback function to return image metadata via
    - `cb` will have the function signature `(err, images)`
    - If there is an error, run `cb(err)`. Otherwise, callback with an array of image metadata (i.e. `cb(null, images)`)
    - This should be called asynchronously (e.g. if creation is synchronous, use `process.nextTick`)
    - image `Object` - Metadata container about corresponding input image at same index
        - height `Number` - Height in pixels of corresponding input image at same index
        - width `Number` - Width in pixels of corresponding input image at same index
        - Any other metadata can be stored here and will be passed to `canvas.addImage` (e.g. `filepath`)

### `new Canvas(width, height, engine)`
Placeholder documentation to suggest how to write a `canvas`. It's possible to avoid using a `constructor` but it will lead to more trouble than not.

The following methods are required as part of the returned `Canvas` object

### `canvas.addImage(image, x, y)`
`Function` to add an image onto our canvas. This should have the function signature `(image, x, y)`

- image `Object` - Image object created via `engine.createImages`
    - This will be the **same** object so any additional metadata will be accessible (e.g. `filepath`)
- x `Number` - Horizontal coordinate to position left edge of image
- y `Number` - Vertical coordinate to position top edge of image

**Note:** This method is intentionally synchronous. Please run all asynchronous actions during `canvas.export`. We suggest saving any critical metadata to `canvas` and reusing it during `canvas.export`.

### `canvas.export(options)`
`Function` to export canvas as an image. This should have the function signature `(options, cb)`

- options `Object` - Modifiers to indicate how to export (e.g. `format`, `quality`)
    - format `String` - Image format to export canvas as
        - Most engines should default to `png`
        - Commonly used formats are: `png`, `jpeg`, and `gif`
    - Any other options can be defined custom to your engine (e.g. `quality`)

**Returns:**

- resultStream `Object` - Readable stream with exported image being written out via `Buffers` on `data` events

## Discovery
In order to make our engines easily discoverable, please provide a `spritesmith-engine` keyword. Here's a list of existing spritesmith engines:

https://www.npmjs.com/browse/keyword/spritesmith-engine

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
