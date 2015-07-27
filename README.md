# the-holy-grail: Gulp + Sass + LiveReload + Foundation

## Goals

To have a gulp workflow that with a single process,

1. watches for any sass changes, then compiles sass source into css
2. watches for any changes in the public directory, triggers live-reload
3. serves your static content in `public/`

### Steps to achieve goals


### make sure you have all required dependencies

_If you have not have npm, gulp, or bower, you need to install them._

_to install npm_
`brew install npm`

_to install gulp_
`npm install -g gulp`

_to install bower_
`npm install -g bower`

### setup gulp + sass + livereload

1. initialize your project with a `package.json` file

  ````
  npm init
  ````
2. setup your `.gitignore` file to ignore `node_modules`

  ````
  echo "node_modules" >> .gitignore
  ````
3. add and commit your 2 new files to be tracked in git
4. install and save required dependencies using `npm install -D [node_module name]`
    1. `gulp`
    2. `gulp-sass`
    3. `gulp-connect`
5. setup sass source directory `mkdir sass/`
6. setup sass source file `touch sass/styles.scss`
7. setup sass compiled output directory `mkdir -p public/css`
8. setup root html5 template file `touch public/index.html`
9. generate a base html5 template in `public/index.html`
10. include a link to `css/styles.css`
11. include an `h1` element in the body with the content of `Hello Gulp, Sass, and Livereload`
12. setup a Gulpfile `touch gulpfile.js`
13. setup the Gulpfile tasks,
  1. `subl gulpfile.js`
    2. [contents of gulpfile.js](http://i.imgur.com/fHy4XRP.png)
14. test gulp task
  1. write a 'hello world' of sass, `subl sass/styles.scss`

    ````
    $dark-color : #333333;

    body {
      background: $dark-color;
    }
    ````
    2. run `gulp` in your terminal
    3. check output, `ls public/css` (should say styles.css)
    4. inspect output, `cat public/css/styles.css` ( should be proper css )
15. test livereload
  1. open a browser `open localhost:8080`
  2. you should see a dark grey background and your single black h1 text
  3. while you terminal, browser, and sublime text are all visible, add the following to your `styles.scss`

  ````
  h1 {
    color: #F1F1F1;
  }
  ````
  4. **_note_** livereload will only track files that it _sees_ when you first start the gulp task. including the initial generated css files, after the first time you compile, you'll have to *restart gulp!*
  5. once you save this file, your browser should automatically refresh, and your h1 element should be kind of white
16. once gulp sass and livereload are wired up correctly, commit your new files
17. make sure to keep your "gulp" terminal running and visible, if your sass source is invalid, it will crash the watching gulp process, and you will want to see the error message that will output in that terminal. Then you can restart your `gulp` watcher after fixing your errors.
18. After adding new files to your `public/` directory (like images or javascript), you may need to kill then restart your gulp task

### setup foundation

1. initialize bower config `bower init`
  say yes to all
2. create a new `.bowerrc` file `subl .bowerrc` and add the following 3 lines to this file

  ````
  {
      "directory" : "public/bower_components"
  }
  ````
3. install foundation, `bower install -S foundation`
4. add `public/bower_components` to you `.gitignore` file

  ````
  echo "public/bower_components" >> .gitignore
  ````
5. create a sass partial subdirectory `mkdir sass/partials`
6. copy over the foundatian *settings* partial, into your `sass/partials` directory

  ````
  cp public/bower_components/foundation/scss/foundation/_settings.scss sass/partials/
  ````
7. create a new partial named `_foundation.scss` in the `sass/partials/` directory

  ````
  subl sass/partials/_foundation.scss
  ````
8. *import* any modules that you need, into the `_foundations.scss` partial, this file will only contain the modules you need to use. refer to the foundation documentation to choose which components you need for your layout. if you want to use the topbar, rtfm on how to use it here [http://foundation.zurb.com/docs/components/topbar.html](http://foundation.zurb.com/docs/components/topbar.html). import each model you need, for example:

  ````
  @import "../public/bower_components/foundation/scss/foundation/components/grid";
  @import "../public/bower_components/foundation/scss/foundation/components/block-grid";
  @import "../public/bower_components/foundation/scss/foundation/components/clearing";
  @import "../public/bower_components/foundation/scss/foundation/components/top-bar";
  @import "../public/bower_components/foundation/scss/foundation/components/type";
  @import "../public/bower_components/foundation/scss/foundation/components/visibility";
  ````
  notice that we updated our paths to the proper partial file locations installed by bower
7. fix the path in our other partial, `_settings.scss`, on *line 58*

  ````
  // change this
  @import "foundation/functions";

  // to this
  @import "../public/bower_components/foundation/scss/foundation/functions";
  ````
8. import your 2 partials into your main `styles.scss` file:
  _at the top_

  ````
  @import "partials/settings";
  @import "partials/foundation";
  ````
9. test that foundation is being properly loaded, it should have autocompiled (sass) if your gulp watcher was on, if not, (stop and) restart it.
  1. review your `public/css/styles.css`
  2. make sure that it is including all of the foundation components that you chose (from `sass/partials/_foundation.scss`), it will be like 1000 to 3000 lines of compiled css.

### final setup

In order for the webpage to render and scale properly on mobile devices, add this meta tag to your `public/index.html` file in the the `<head>` element

````
 <meta name="viewport" content="width=device-width, initial-scale=1.0" />
````

To take advantage of feature detection, include the [modernizr](http://modernizr.com/) plugin,
add this script tag to your `public/index.html` file at the end of the `<body>` element, before any of _your_ javascript files

````
 <script src="/bower_components/modernizr/modernizr.js"></script>
````

Include the required [jquery](http://jquery.com/) and [foundation](http://foundation.zurb.com/) javascript libraries (right after the modernizr include)

````
 <script src="/bower_components/jquery/dist/jquery.min.js"></script>
 <script src="/bower_components/foundation/js/foundation.min.js"></script>
 <script>
   $(document).foundation();
 </script>
````

### the end