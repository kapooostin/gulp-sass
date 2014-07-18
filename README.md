[![Build Status](https://travis-ci.org/dlmanning/gulp-sass.svg?branch=master)](https://travis-ci.org/dlmanning/gulp-sass)

gulp-sass
=========

SASS plugin for [gulp](https://github.com/gulpjs/gulp).

#Install

```
npm install gulp-sass
```

#Basic Usage

Something like this:

```javascript
var gulp = require('gulp');
var sass = require('gulp-sass')

gulp.task('sass', function () {
	gulp.src('./scss/*.scss')
		.pipe(sass())
		.pipe(gulp.dest('./css'));
});
```

Options passed as a hash into `sass()` will be passed along to [`node-sass`](https://github.com/sass/node-sass)

## gulp-sass specific options

#### `errLogToConsole: true`

If you pass `errLogToConsole: true` into the options hash, sass errors will be logged to the console instead of generating a `gutil.PluginError` object. Use this option with `gulp.watch` to keep gulp from stopping every time you mess up your sass.

#### `onSuccess: callback`

Pass in your own callback to be called upon successful compilaton by node-sass. The callback has the form `callback(css)`, and is passed the compiled css as a string. Note: This *does not* prevent gulp-sass's default behavior of writing the output css file.

#### `onError: callback`

Pass in your own callback to be called upon a sass error from node-sass. The callback has the form `callback(err)`, where err is the error string generated by libsass. Note: this *does* prevent an actual `gulpPluginError` object from being created.

## Source Maps

gulp-sass now generates *inline* source maps if you pass `sourceComments: 'map'` as an option. Note that gulp-sass won't actually do anything when passing `sourceMap: filepath`. Enjoy your source maps!

NB: For those wondering, inline source maps are stuck onto the end of the css file instead of being in a separate map file. In this case, the original source contents are included as well, so you don't have to make sure your scss files are servable.

#Imports and Partials

gulp-sass now automatically passes along the directory of every scss file it parses as an include path for node-sass. This means that as long as you specify your includes relative to path of your scss file, everything will just work.

scss/includes/_settings.scss:

```scss
$blue: #3bbfce;
$margin: 16px;
```

scss/style.scss:

```scss
@import "includes/settings";

.content-navigation {
  border-color: $blue;
  color:
    darken($blue, 9%);
}

.border {
  padding: $margin / 2;
  margin: $margin / 2;
  border-color: $blue;
}
```

