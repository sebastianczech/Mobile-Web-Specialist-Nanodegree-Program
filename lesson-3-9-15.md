# Lesson 3.9 - Introducing Web Tooling and Automation

* Use libraries instead of building something from scratch.
* Use web tools, which have great community.
* There is no one library, which has everything you need.
* Be careful with micro optimizations e.g .speeding launching IDE by 10%, which is started once a day.

# Lesson 3.10 - Productive Editing

* IDE - Integrated Development Environment
* In IDE use build-in shortcuts and features.
* Extend your IDE by installing plugins and packges to editor.

# Lesson 3.11 - Powerful Builds

* Build tools:
   * for every projects: shell scripts,
   * for Java: Ant, Maven, Gradle,
   * for web: Grunt, [Gulp](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md).
* Why use build tools ? 
   * fast,
   * community-driven,
   * modular and extensible,
   * feature-rich.
* Advantages of Gulp over Grunt:
   * built for speed and can execute tasks in parallel,
   * converts open files into super fast streams internally.
* Gulp's taks use code over configuration by using JavaScript.

* Example of ```gulpfile```:

```JavaScript
var gulp = require('gulp');

gulp.task('default', defaultTask);

function defaultTask(done) {
  // place code for your default task here
  done();
}
```

* Tools for CSS: LESS, [SASS](http://sass-lang.com/)
* [Sass plugin for Gulp](https://www.npmjs.com/package/gulp-sass)

* Example of using SASS plugin:

```JavaScript
'use strict';
 
var gulp = require('gulp');
var sass = require('gulp-sass');
 
gulp.task('sass', function () {
  return gulp.src('./sass/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});
 
gulp.task('sass:watch', function () {
  gulp.watch('./sass/**/*.scss', ['sass']);
});
```

* [Prefix CSS (e.g. for last 2 versions of all browsers) with Autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer):

```JavaScript
const gulp = require('gulp');
const autoprefixer = require('gulp-autoprefixer');
 
gulp.task('default', () =>
    gulp.src('src/app.css')
        .pipe(autoprefixer({
            browsers: ['last 2 versions'],
            cascade: false
        }))
        .pipe(gulp.dest('dist'))
);
```

# Lesson 3.12 - Expressive Live Editing

* [Chrome Dev Workspaces](https://developers.google.com/web/tools/setup/setup-workflow)
* [Browser Sync](https://browsersync.io/)

* Create a browser-sync object and initialize the server:

```JavaScript
var browserSync = require('browser-sync').create();
 browserSync.init({
     server: "./"
 });
 browserSync.stream();
```

# Lesson 3.13 - How to Prevent Disasters

* [Comparison of JavaScript Linting Tools](https://www.sitepoint.com/comparison-javascript-linting-tools/)
* [ESLint - The pluggable linting utility for JavaScript and JSX](https://eslint.org/)

* Linting via:
   * IDE (live linting)
   * build process
   * pre-commit hook in version control   
   
* Linting:
   * code style
   * syntax
   
* Popular linters:
   * JSHint
   * JSCS
   * ESLint
   
* [gulp-eslint](https://www.npmjs.com/package/gulp-eslint):

```JavaScript
const gulp = require('gulp');
const eslint = require('gulp-eslint');
 
gulp.task('lint', () => {
    // ESLint ignores files with "node_modules" paths.
    // So, it's best to have gulp ignore the directory as well.
    // Also, Be sure to return the stream from the task;
    // Otherwise, the task may end before the stream has finished.
    return gulp.src(['**/*.js','!node_modules/**'])
        // eslint() attaches the lint output to the "eslint" property
        // of the file object so it can be used by other modules.
        .pipe(eslint())
        // eslint.format() outputs the lint results to the console.
        // Alternatively use eslint.formatEach() (see Docs).
        .pipe(eslint.format())
        // To have the process exit with an error code (1) on
        // lint error, return the stream and pipe to failAfterError last.
        .pipe(eslint.failAfterError());
});
 
gulp.task('default', ['lint'], function () {
    // This will only run if the lint task is successful...
});
```
   
* [Udacity JavaScript Testing course](https://eu.udacity.com/course/javascript-testing--ud549)

* [Jasmine - Behavior-Driven JavaScript](https://jasmine.github.io/)
* [gulp-jasmine-phantom](https://www.npmjs.com/package/gulp-jasmine-phantom):

```JavaScript
var gulp = require('gulp');
var jasmine = require('gulp-jasmine-phantom');
 
gulp.task('default', function () {
  return gulp.src('spec/test.js')
          .pipe(jasmine());
});
```

# Lesson 3.14 - Awesome Optimizations

* Make ```index.html``` automatically reload:

```JavaScript
gulp.watch("./build/index.html").on('change', browserSync.reload);
```

* SASS is doing concatenation and minification for CSS:

```JavaScript
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
   .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
   .pipe(gulp.dest('./css'));
});
```

* [JavaScript concatenation](https://www.npmjs.com/package/gulp-concat):

```JavaScript
var concat = require('gulp-concat');
 
gulp.task('scripts', function() {
  return gulp.src('./lib/*.js')
    .pipe(concat('all.js'))
    .pipe(gulp.dest('./dist/'));
});
```

* [JavaScript minification](https://www.npmjs.com/package/gulp-uglify):

```JavaScript
var concat = require('gulp-concat');
 
gulp.task('scripts', function() {
  return gulp.src('./lib/*.js')
    .pipe(concat('all.js'))
    .pipe(uglify())
    .pipe(gulp.dest('./dist/'));
});
```

```JavaScript
var gulp = require('gulp');
var uglify = require('gulp-uglify');
var pump = require('pump');
 
gulp.task('compress', function (cb) {
  pump([
        gulp.src('lib/*.js'),
        uglify(),
        gulp.dest('dist')
    ],
    cb
  );
});
```

* [https://css-tricks.com/the-difference-between-minification-and-gzipping/](https://css-tricks.com/the-difference-between-minification-and-gzipping/)

* [Babel - a JavaScript compiler.](https://babeljs.io/)

* [Gulp Babel](https://www.npmjs.com/package/gulp-babel):

```JavaScript
const gulp = require('gulp');
const babel = require('gulp-babel');
 
gulp.task('default', () =>
    gulp.src('src/app.js')
        .pipe(babel({
            presets: ['env']
        }))
        .pipe(gulp.dest('dist'))
);
```

* [Source maps](https://www.npmjs.com/package/gulp-sourcemaps) are files that associate line numbers from the processed file to the original. This way the browser can lookup the current line number in the sourcemap and open the right source file at the correct line when debugging. In Chrome for instance, the DevTools support source maps both for CSS and JavaScript:

```JavaScript
var gulp = require('gulp');
var plugin1 = require('gulp-plugin1');
var plugin2 = require('gulp-plugin2');
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('javascript', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.init())
      .pipe(plugin1())
      .pipe(plugin2())
    .pipe(sourcemaps.write())
    .pipe(gulp.dest('dist'));
});
```

* [Plugins with gulp sourcemaps support](https://github.com/gulp-sourcemaps/gulp-sourcemaps/wiki/Plugins-with-gulp-sourcemaps-support)

* [Introduction to JavaScript Source Maps](https://www.html5rocks.com/en/tutorials/developertools/sourcemaps/)

* Image optimization courses:
   * [Browser Rendering Optimization](https://www.udacity.com/course/browser-rendering-optimization--ud860)
   * [Web Performance Optimization](https://www.udacity.com/course/website-performance-optimization--ud884)
   * [Responsive Web Design](https://www.udacity.com/course/responsive-web-design-fundamentals--ud893)
   * [Responsive Images](https://www.udacity.com/course/responsive-images--ud882)
   
* Image compression with [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin):

```JavaScript
const gulp = require('gulp');
const imagemin = require('gulp-imagemin');
 
gulp.task('default', () =>
    gulp.src('src/images/*')
        .pipe(imagemin())
        .pipe(gulp.dest('dist/images'))
);
```

* [PNG Quantization](https://www.npmjs.com/package/imagemin-pngquant):

```JavaScript
const imagemin = require('imagemin');
const imageminPngquant = require('imagemin-pngquant');
 
imagemin(['images/*.png'], 'build/images', {use: [imageminPngquant()]}).then(() => {
    console.log('Images optimized');
});
```

```JavaScript
gulp.task('default', function() {
    return gulp.src('src/images/*')
        .pipe(imagemin({
            progressive: true,
            use: [pngquant()]
        }))
        .pipe(gulp.dest('dist/images'));
});
```

# Lesson 3.15 - Web Tooling and Automation Conclusion

TODO
