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

### Structure
A spritesmith `engine` returns the following properties on its `module.exports`:

- specVersion `String` - Current [semver][] the engine is supporting (e.g. '1.0.0')
-


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
