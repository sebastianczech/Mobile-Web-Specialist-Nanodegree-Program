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

TODO

# Lesson 3.13 - How to Prevent Disasters

TODO

# Lesson 3.14 - Awesome Optimizations

TODO

# Lesson 3.15 - Web Tooling and Automation Conclusion

TODO
