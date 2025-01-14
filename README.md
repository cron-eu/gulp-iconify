gulp-iconify
============

We're keeping this only for the sake of maintaining legacy projects!!

Don't use it for new projects!!

## MaxYari's changes

You can provide `false` to `.cssOutput` and/or `.pngOutput` to disable scss->css and png generation, leaving only svg->svss generation, which is an only thing from this package that I personally use.
Svg parser now also attempts to pull width/height data from `viewBox` property of svg, which is imo used quite often, if not more often than width/height.

## Original readme

'A mystical CSS icon solution', [grunticon](https://github.com/filamentgroup/grunticon)-like build system for [Gulp](https://github.com/gulpjs/gulp):

'~~grunticon is a Grunt.js~~ gulp-iconify is a gulp task that makes it easy to manage icons and background images for all devices, preferring HD (retina) SVG icons but also provides fallback support for standard definition browsers, and old browsers alike. From a CSS perspective, it's easy to use, as it generates a class referencing each icon, and doesn't use CSS sprites.'


## Usage
```shell
npm install gulp-iconify --save-dev
```

### Simple example

```javascript
gulp.task('default', function() {
    iconify({
        src: './img/icons/*.svg'
    });
});
```

This simple call defaults to the following:

- SVGs will be passed through SVGO (and optimised)
- Rendered PNGs will be saved in: './img/icons/png'
- Rendered SCSS files will NOT be saved
- Rendered CSS files will be saved in: './css'
- If SVG has no width attribute, the default fallback will be 300px
- If SVG has no height attribute, the default fallback will be 200px
- The default styleTemplate fill be used (examples shown below)
- The default styleTemplate will *not* use the height/width slugs
- See [gulp-svg2png](https://github.com/akoenig/gulp-svg2png) for default settings

### Customized example

```javascript
gulp.task('default', function() {
    iconify({
        src: './img/icons/*.svg',
        pngOutput: './img/icons/png',
        scssOutput: './scss',
        cssOutput:  './css',
        styleTemplate: '_icon_gen.scss.mustache',
        defaultWidth: '300px',
        defaultHeight: '200px',
        svgoOptions: {
            enabled: true,
            options: {
                plugins: [
                    { removeUnknownsAndDefaults: false },
                    { mergePaths: false }
                ]
            }
        },
        svg2pngOptions: {
            scaling: 1.0,
            verbose: true,
            concurrency: null
        }
    });
});
```

Note: To disable SVGO, just set ```svgoOptions: { enabled: ___ }``` to anything but ```true``` .

### Example (and default) styleTemplate:

```mustache
.icon {
    background-repeat: no-repeat;

    {{#items}}
    &.icon-{{slug}} {
        background-image: url('{{{datauri}}}');
    }

    {{/items}}
}
```

### Example styleTemplate with height/width slugs:

```mustache
.icon {
    background-repeat: no-repeat;

    {{#items}}
    &.icon-{{slug}} {
        background-image: url('{{{datauri}}}');
        width: {{width}}px;
        height: {{height}}px;
    }

    {{/items}}
}
```
