# Trianglify [![Build Status](https://travis-ci.org/qrohlf/trianglify.svg?branch=master)](https://travis-ci.org/qrohlf/trianglify)


Trianglify is a library that I wrote to generate nice SVG background images like this one:

![](https://cloud.githubusercontent.com/assets/347189/6771063/f8b0af46-d090-11e4-8d4c-6c7ef5bd9d37.png)

It was inspired by [btmills/geopattern](https://github.com/btmills/geopattern) and the initial version was written in a single day because I got fed up with Adobe Illustrator.

Version 0.2.0 represents a ground-up rewrite of the original which eliminates the dependency on d3.js, streamlines rendering, and reworks the API for consistiency and ease-of-use.

*v0.1.x users should note that the v0.2.0 API has changed significantly and is not backwards-compatible*

# Getting Trianglify

You can grab Trianglify with your preferred package manager:

```
npm install trianglify
bower install trianglify
```

Include it in your HTML via CDNJS:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/trianglify/0.2.1/trianglify.min.js"></script>
```

Or clone the repo:

```
git clone https://github.com/qrohlf/trianglify.git
```


# Quickstart

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/trianglify/0.2.0/trianglify.min.js"></script>
<script>
	var pattern = Trianglify({
		width: window.innerWidth, 
		height: window.innerHeight
	});
	document.body.appendChild(pattern.canvas())
</script>
```

See https://qrohlf.com/trianglify for interactive examples and a walkthrough of the most commonly-used Trianglify options.


# API

Trianglify exposes a single function into the global namespace, called `Trianglify`. This takes a single options object as an argument and returns a pattern object.

```js
var Trianglify = require('trianglify'); // only needed in node.js
var pattern = Trianglify({width: 200, height: 200})
```

The pattern object contains data about the generated pattern's options and geometry, as well as rending implementations.

### pattern.opts

Object containing the options used to generate the pattern.

### pattern.polys

The colors and vertices of the polygons that make up the pattern, in the following format:

```js
[
  ['color', [vertex, vertex, vertex]],
  ['color', [vertex, vertex, vertex]],
  ...
]
```

### pattern.svg()

Rendering function for SVG. Returns an SVGElement DOM node.

### pattern.canvas([HTMLCanvasElement])

Rendering function for canvas. When called with no arguments returns a HTMLCanvasElement DOM node. When passed an existing canvas element as an argument, renders the pattern to the existing canvas.

### pattern.png()

Rendering function for PNG. Returns a data URI with the PNG data in base64 encoding. See [examples/save-as-png.js](examples/save-as-png.js) for an example of decoding this into a file.


# Options

Trianglify is configured by an options object passed in as the only argument. The following option keys are supported:

### width

Integer, defaults to `600`. Specify the width in pixels of the pattern to generate. 

### height

Integer, defaults to `400`. Specify the height in pixels of the pattern to generate.

### cell_size

Integer, defaults to `75`. Specify the size of the mesh used to generate triangles. Larger values will result in coarser patterns, smaller values will result in finer patterns. Note that very small values may dramatically increase the runtime of Trianglify.

### variance

Decimal value between 0 and 1 (inclusive), defaults to 0.75. Specify the amount of randomness used when generating triangles. 

### seed

Number or string, defaults to `null`. Seeds the random number generator to create repeatable patterns. When set to null, the random number will be seeded with random values from the environment. An example usage would be passing in blog post titles as the seed to generate unique trianglify patterns for every post on a blog that won't change when the page reloads.

### x_colors

String or array of CSS-formatted colors, default is `'random'`. Specify the color gradient used on the x axis.

Valid string values are 'random' or the name of a [colorbrewer palette](http://bl.ocks.org/mbostock/5577023) (i.e. 'YlGnBu' or 'RdBu'). When set to 'random', a gradient will be randomly selected from the colorbrewer library.

Valid array values should specify the color stops in any CSS format (i.e. `['#000000', '#4CAFE8', '#FFFFFF']`).

### y_colors

String or array of CSS-formatted colors, default is `'match_x'`. When set to 'match_x' the same gradient will be used on both axes. Otherwise, accepts the same options as x_colors.

### color_space

String, defaults to `'lab'`. Set the color space used for generating gradients. Supported values are rgb, hsv, hsl, hsi, lab and hcl. See this [blog post](https://vis4.net/blog/posts/avoid-equidistant-hsv-colors/) for some background on why this matters.

### color_function

Specify a custom function for coloring triangles, defaults to `null`. Accepts a function to override the standard gradient coloring that takes the x,y coordinates of a triangle's centroid as arguments and returns a CSS-formatted color string representing the color that triangle should have.

Here is an example color function that uses the HSL color format to generate a rainbow pattern:

```javascript
var colorFunc = function(x, y) {
	return 'hsl('+Math.floor(Math.abs(x*y)*360)+',80%,60%)';
};

var pattern = Trianglify({color_function: colorFunc})
```

### stroke_width

Number, defaults to `1.51`. Specify the width of the stroke on triangle shapes in the pattern. The default value is the ideal value for eliminating antialiasing artifacts when rendering patterns to a canvas.

# License

Trianglify is GPLv3 licensed. Please [contact me](mailto:qr@qrohlf.com) if you are interested in purchasing Trianglify under an alternative license.