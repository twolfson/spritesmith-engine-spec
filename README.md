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

- specVersion `String` - Current [semver][] the engine is supporting (e.g. '1.0.0')
- createCanvas `Function` - Utility to create `canvas` for `engine`
- createImages `Function` - Utility to create images which will later be laid out via a `canvas`

### `engine.createCanvas(width, height, cb)`
`engine.createCanvas` should have the function signature `(width, height, cb)`

- width `Number` - Width in pixels for the canvas. Upon `export`, the output image should have this `width`
- height `Number` - Height in pixels for the canvas. Upon `export`, the output image should have this `height`
- cb `Function` - Error-first callback function to return canvas via
    - `cb` will have the function signature `(err, canvas)`
    - If there is an error, run `cb(err)`. Otherwise, callback with a canvas (i.e. `cb(null, canvas)`)
    - This should be called asynchronously (e.g. use `process.nextTick` for after synchronous creation)

### `engine.createImages(images, cb)`
`engine.createImages` should have the function signature `(images, cb)`

- images `String[]` - Array of filepaths to images
- cb `Function` - Error-first callback function to return image metadata via
    - `cb` will have the function signature `(err, images)`
    - If there is an error, run `cb(err)`. Otherwise, callback with an array of image metadata (i.e. `cb(null, images)`)
    - This should be called asynchronously (e.g. use `process.nextTick` for after synchronous creation)
    - An `image` (i.e. an item from `images`) should have the following structure
        - height `Number` - Height in pixels of corresponding input image at same index
        - width `Number` - Width in pixels of corresponding input image at same index
        - Any other metadata can be stored here and will be passed to `canvas.addImage` (e.g. `filepath`)

### canvas structure


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
