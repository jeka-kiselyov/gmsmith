# gmsmith [![Build status](https://travis-ci.org/twolfson/gmsmith.png?branch=master)](https://travis-ci.org/twolfson/gmsmith)

[GM][gm] engine for [spritesmith][].

[gm]: http://aheckmann.github.io/gm/
[spritesmith]: https://github.com/Ensighten/spritesmith

## Requirements
`gmsmith` depends on [gm][] which depends on [GraphicsMagick][].

I have found it is best to install from the site rather than through a package manager (e.g. `apt-get`) to get the latest as well as without transparency issues.

This module has been developed and tested against `1.3.17`.

> Alertnatively, you can use [ImageMagick][] which is implicitly discovered if `gm` is not installed.
> http://www.imagemagick.org/script/index.php

[ImageMagick]: http://www.imagemagick.org/script/index.php
[GraphicsMagick]: http://www.graphicsmagick.org/

## Getting Started
Install the module with: `npm install gmsmith`

```js
// Load in our dependencies
var Gmsmith = require('gmsmith');

// Create a new engine
var gmsmith = new Gmsmith();

// Interpret some images from disk
gmsmith.createImages(['img1.jpg', 'img2.png'], function handleImages (err, imgs) {
  // If there was an error, throw it
  if (err) {
    throw err;
  }

  // We recieve images in the same order they were given
  imgs[0].width; // 50 (pixels)
  imgs[0].height; // 100 (pixels)

  // Create a canvas that fits our images (200px wide, 300px tall)
  var canvas = gmsmith.createCanvas(200, 300);

  // Add the images to our canvas (at x=0, y=0 and x=50, y=100 respectively)
  canvas.addImage(imgs[0], 0, 0);
  canvas.addImage(imgs[1], 50, 100);

  // Export canvas to image
  var resultStream = canvas['export']({format: 'png'});
  resultStream; // Readable stream outputting PNG image of the canvas
});
```

## Documentation
This module was built to the specification for spritesmith engines.

**Specification version:** 2.0.0

https://github.com/twolfson/spritesmith-engine-spec/tree/2.0.0

### `new Engine(options)`
This is also known as `new Gmsmith`.

Our `Engine` constructor provides support for the following options:

- options `Object`
    - imagemagick `Boolean` - Flag to indicate whether to use [ImageMagick][] over [GraphicsMagick][]
        - When `true`, [ImageMagick][] will be used. Otherwise, [implicit discovery](#requirements) will be used.

### `engine.createImages(images, cb)`
Our `createImages` methods supports the following types of images:

- image `String` - Filepath to image
- image `Object` - Vinyl object with `null` for image (uses provided filepath)

We accept but don't fully support the following types of images:

- image `Object` - Vinyl object with buffer for image (ignores contents, uses provided filepath)
- image `Object` - Vinyl object with stream for image (ignores contents, uses provided filepath)

### `canvas.export(options)`
Our `export` method provides support for the following options:

- options `Object`
    - format `String` - Output image format to callback with. Currently, png and jpeg are available.
    - quality `Number` - Quality of output image on a scale from 0 to 100
    - progressive `Boolean` - Make output image progressive. Works with jpeg format only.

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint using `npm run lint` and test via `npm test`.

## Donating
Support this project and [others by twolfson][twolfson-projects] via [donations][twolfson-support-me].

<http://twolfson.com/support-me>

[twolfson-projects]: http://twolfson.com/projects
[twolfson-support-me]: http://twolfson.com/support-me

## License
Copyright (c) 2013 Todd Wolfson

Licensed under the MIT license.
