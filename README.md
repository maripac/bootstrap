#ABOUT THIS REPO

## clone repo and install dependencies


Clone bootstrap's repo, cd into the directory storing the package.json and Gruntfile.js, and install node modules.


`git clone https://github.com/maripac/bootstrap.git`

`cd bootstrap`

`npm install`



## bootstrap.less


Inside the less folder found at the root of the project there is a bootstrap.less file which contains the `@imports` to all the components of the framework. I only need the grid system, so the list below are the only lines I leave uncommented:

*  **@import "variables.less";**
*  **@import "mixins.less";**
*  **@import "scaffolding.less";**
*  **@import "grid.less";**
*  @import "utilities.less";
*  @import "responsive-utilities.less";

The variables.less file defines a collection of variables, the associated values are used to process the operations defined inside the mixins folder when we compile our stylesheets. We can modify these values as we prefer.



## variables.less

### Predefine views before I edit variables.less 

To predefine the views, the reference I'll use is the 960 grid system. With 10px padding to the right and left of the container which leaves an inner container of 940 pixels of width. The 12 columns set to 60px of width leaves 220px of remaining space that divided into 11 gutters gives 20px of gutter width between every pair of columns. With this distribution as a reference the predefined 5 views will be calculated as follows:

|         | col-lg         | col-md          | col-sm       | col-xs        | col-xxs      |
|---------|---------------:|----------------:|-------------:|--------------:|-------------:|
|  col    | 16 x 60 = 960  | 12 x 60 = 720   | 9 x 60 = 540 | 6 x 60 = 360  | 4 x 60 = 240 |
| gutter  | 15 x 20 = 300  | 11 x 20 = 220   | 8 x 20 = 160 | 5 x 20 = 100  | 3 x 20 = 60  |
| padding | 10 x 2 = 20    | 10 x 2 = 20     | 10 x 2 = 20  | 10 x 2 = 20   | 10 x 2 = 20  |
| total   | 1280px         | 960px           | 720px        | 480px         | 320px        |


Now I have the values I'll be using for the gutters and for the containers div of my predefined views. These values will be used as well as the values of the breakpoints that will trigger the transition between the views
in the css media rules. I've intentionally simplified the calculation of the breakpoints because I prefer to set up the resizing of the layout with attendance to a proportional coherence, instead of targeting standard device's widths.  

_note: The number of columns is used only as a reference. I'll be working with bootstrap's default number of columns (which is 12) for every screen size I predefine. The reason is that bootstrap allows the nesting of rows of columns inside a parent column. Only, it is important to remember to insert between the parent column and the nested columns a row div, that adds a negative padding between the parent and children columns, avoiding the nested padding effect._

### Edit variables.less

Around line 260 add:


```
//== Media queries breakpoints

@screen-xxs:                  320px;
@screen-xxs-min:              @screen-xxs;
@screen-small-phone:          @screen-xxs-min;
```
Edit the following lines:

`@screen-xs:                  480px;`

`@screen-sm:                  768px;`

`@screen-md:                  992px;`

`@screen-lg:                  1200px;`


Leave them as follows:

`@screen-xs:                  480px;`

`@screen-sm:                  720px;`

`@screen-md:                  960px;`

`@screen-lg:                  1280px;`


Add a new line:

`@screen-xxs-max:             (@screen-xs-min - 1);`


Set the @grid-gutter-width value to 20px instead of 30px:

`@grid-gutter-width:         20px;`


In the Container sizes section add:

```
@container-small-phone:        ((300px + @grid-gutter-width));
@container-xxs:                @container-small-phone;

@container-phone:              ((460px + @grid-gutter-width));

@container-xs:                  @container-phone;
```

Set the @container-tablet value to 700px, instead of 720px:

`@container-large-desktop:      ((700px + @grid-gutter-width));`


Set the @container-large-desktop value to 1260px, instead of 1140px:

`@container-large-desktop:      ((1260px + @grid-gutter-width));`



## grid.less

Inside the container block add the media rule for the container-xs


```
.container {
  .container-fixed();

  @media (min-width: @screen-xs-min) {
  width: @container-xs;
}

  @media (min-width: @screen-sm-min) {
    width: @container-sm;
  }
  @media (min-width: @screen-md-min) {
    width: @container-md;
  }
  @media (min-width: @screen-lg-min) {
    width: @container-lg;
  }
}
```

Add the following lines:

```

@media (min-width: @screen-xxs-min) {
  .make-grid(xxs);
}


@media (min-width: @screen-xs-min) {
  .make-grid(xs);
}
```

~~Note: I haven't tried with this (might be better approach)~~ 

This is a better approach 

```
.make-grid(xxs);

@media (min-width: @screen-xs-min) {
  .make-grid(xs);
}
```
 
## Inside the mixins folder

### grid.less


Edit the next block:

```
// Generate the extra small columns
.make-xs-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
  float: left;
  width: percentage((@columns / @grid-columns));
  min-height: 1px;
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);
}
.make-xs-column-offset(@columns) {
  margin-left: percentage((@columns / @grid-columns));
}
.make-xs-column-push(@columns) {
  left: percentage((@columns / @grid-columns));
}
.make-xs-column-pull(@columns) {
  right: percentage((@columns / @grid-columns));
}
```

Leave it as follows:

```
// Generate the extra small columns
.make-xs-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
 // float: left;
 // width: percentage((@columns / @grid-columns));
  min-height: 1px;
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);
    @media (min-width: @screen-xs-min) {
    float: left;
    width: percentage((@columns / @grid-columns));
  }
}
.make-xs-column-offset(@columns) {
  @media (min-width: @screen-xs-min) {
    margin-left: percentage((@columns / @grid-columns));
  }
  
}
.make-xs-column-push(@columns) {
  @media (min-width: @screen-xs-min) {
    left: percentage((@columns / @grid-columns));
  }
  
}
.make-xs-column-pull(@columns) {
  
  @media (min-width: @screen-xs-min) {
    right: percentage((@columns / @grid-columns));
  }
}
```

Add before the edited block of code the following:

```
// Generate the extra extra small columns
.make-xxs-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
  float: left;
  width: percentage((@columns / @grid-columns));
  min-height: 1px;
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);
}
.make-xxs-column-offset(@columns) {
  margin-left: percentage((@columns / @grid-columns));
}
.make-xxs-column-push(@columns) {
  left: percentage((@columns / @grid-columns));
}
.make-xxs-column-pull(@columns) {
  right: percentage((@columns / @grid-columns));
}
```


### grid-framework.less

Edit this block of code:

```
.make-grid-columns() {
  // Common styles for all sizes of grid columns, widths 1-12
  .col(@index) when (@index = 1) { // initial
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), @item);
  }
  .col(@index, @list) when (@index =< @grid-columns) { // general; "=<" isn't a typo
    @item: ~".col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }
  .col(@index, @list) when (@index > @grid-columns) { // terminal
    @{list} {
      position: relative;
      // Prevent columns from collapsing when empty
      min-height: 1px;
      // Inner gutter via padding
      padding-left:  (@grid-gutter-width / 2);
      padding-right: (@grid-gutter-width / 2);
    }
  }
  .col(1); // kickstart it
}
```
 
Leave it as follows:

```
.make-grid-columns() {
  // Common styles for all sizes of grid columns, widths 1-12
  .col(@index) when (@index = 1) { // initial
    @item: ~".col-xxs-@{index}, .col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), @item);
  }
  .col(@index, @list) when (@index =< @grid-columns) { // general; "=<" isn't a typo
    @item: ~".col-xxs-@{index}, .col-xs-@{index}, .col-sm-@{index}, .col-md-@{index}, .col-lg-@{index}";
    .col((@index + 1), ~"@{list}, @{item}");
  }
  .col(@index, @list) when (@index > @grid-columns) { // terminal
    @{list} {
      position: relative;
      // Prevent columns from collapsing when empty
      min-height: 1px;
      // Inner gutter via padding
      padding-left:  (@grid-gutter-width / 2);
      padding-right: (@grid-gutter-width / 2);
    }
  }
  .col(1); // kickstart it
}
```



## compile stylesheets

Once I saved the changes from the root folder where the Gruntfile.js is stored enter

`grunt dist`

If there are no errors the compiled css is inside the dist folder.


## Integrate in sass

As I'm working with sass. I add the compiled css in the partials folder as _grid.scss. To visualize the simulated columns, and checkout that the resizing is ok, I would create another partial to setup the vertical distribution. I could add the following .scss mixin at the header of the _vertical_rythm.scss and include it  inside the hover state of the .container class defined in the grid file. This way we should visualize the resizing of the layout corresponds to the table above, for each view.

```

$baseline: 24px;

@mixin baseline-grid {
  $columns: 12;
  $column-color: rgba(200,0,0,.2);
  $baseline-color: rgba(56,255,255,.8);
  
  // These are all automatically calculated
  $gutter-width: 20px;  // Change if you like
  $gutters: ($columns - 1);
  $column-width: 60px;
  
  background-image: -webkit-linear-gradient(0deg, $column-color $column-width, transparent $gutter-width),
              -webkit-linear-gradient(top, rgba(0,0,0,0) 95%, $baseline-color 100%);
  background-image: -moz-linear-gradient(0deg, $column-color $column-width, transparent $gutter-width),
              -moz-linear-gradient(top, rgba(0,0,0,0) 95%, $baseline-color 100%);
  background-image: -o-linear-gradient(0deg, $column-color $column-width, transparent $gutter-width),
              -o-linear-gradient(top, rgba(0,0,0,0) 95%, $baseline-color 100%);
  background-size: 80px 100%,100% 21px;
  background-position: 10px 0px; // Use to offset and center your grid
} 

.container{
  &:hover
    {
      @include baseline-grid;
    }
}
```








# [Bootstrap](http://getbootstrap.com)
[![Bower version](https://badge.fury.io/bo/bootstrap.svg)](http://badge.fury.io/bo/bootstrap)
[![NPM version](https://badge.fury.io/js/bootstrap.svg)](http://badge.fury.io/js/bootstrap)
[![Build Status](https://secure.travis-ci.org/twbs/bootstrap.svg?branch=master)](http://travis-ci.org/twbs/bootstrap)
[![devDependency Status](https://david-dm.org/twbs/bootstrap/dev-status.svg)](https://david-dm.org/twbs/bootstrap#info=devDependencies)
[![Selenium Test Status](https://saucelabs.com/browser-matrix/bootstrap.svg)](https://saucelabs.com/u/bootstrap)

Bootstrap is a sleek, intuitive, and powerful front-end framework for faster and easier web development, created by [Mark Otto](http://twitter.com/mdo) and [Jacob Thornton](http://twitter.com/fat), and maintained by the [core team](https://github.com/twbs?tab=members) with the massive support and involvement of the community.

To get started, check out <http://getbootstrap.com>!

## Table of contents

 - [Quick start](#quick-start)
 - [Bugs and feature requests](#bugs-and-feature-requests)
 - [Documentation](#documentation)
 - [Contributing](#contributing)
 - [Community](#community)
 - [Versioning](#versioning)
 - [Creators](#creators)
 - [Copyright and license](#copyright-and-license)

## Quick start

Three quick start options are available:

- [Download the latest release](https://github.com/twbs/bootstrap/archive/v3.2.0.zip).
- Clone the repo: `git clone https://github.com/twbs/bootstrap.git`.
- Install with [Bower](http://bower.io): `bower install bootstrap`.

Read the [Getting started page](http://getbootstrap.com/getting-started/) for information on the framework contents, templates and examples, and more.

### What's included

Within the download you'll find the following directories and files, logically grouping common assets and providing both compiled and minified variations. You'll see something like this:

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.min.css
│   ├── bootstrap-theme.css
│   └── bootstrap-theme.min.css
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    └── glyphicons-halflings-regular.woff
```

We provide compiled CSS and JS (`bootstrap.*`), as well as compiled and minified CSS and JS (`bootstrap.min.*`). Fonts from Glyphicons are included, as is the optional Bootstrap theme.



## Bugs and feature requests

Have a bug or a feature request? Please first read the [issue guidelines](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md#using-the-issue-tracker) and search for existing and closed issues. If your problem or idea is not addressed yet, [please open a new issue](https://github.com/twbs/bootstrap/issues/new).


## Documentation

Bootstrap's documentation, included in this repo in the root directory, is built with [Jekyll](http://jekyllrb.com) and publicly hosted on GitHub Pages at <http://getbootstrap.com>. The docs may also be run locally.

### Running documentation locally

1. If necessary, [install Jekyll](http://jekyllrb.com/docs/installation) (requires v2.1.x).
  - **Windows users:** Read [this unofficial guide](https://github.com/juthilo/run-jekyll-on-windows/) to get Jekyll up and running without problems.
2. Install the Ruby-based syntax highlighter, [Rouge](https://github.com/jneen/rouge), with `gem install rouge`.
3. From the root `/bootstrap` directory, run `jekyll serve` in the command line.
4. Open <http://localhost:9001> in your browser, and voilà.

Learn more about using Jekyll by reading its [documentation](http://jekyllrb.com/docs/home/).

### Documentation for previous releases

Documentation for v2.3.2 has been made available for the time being at <http://getbootstrap.com/2.3.2/> while folks transition to Bootstrap 3.

[Previous releases](https://github.com/twbs/bootstrap/releases) and their documentation are also available for download.



## Contributing

Please read through our [contributing guidelines](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md). Included are directions for opening issues, coding standards, and notes on development.

Moreover, if your pull request contains JavaScript patches or features, you must include relevant unit tests. All HTML and CSS should conform to the [Code Guide](http://github.com/mdo/code-guide), maintained by [Mark Otto](http://github.com/mdo).

Editor preferences are available in the [editor config](https://github.com/twbs/bootstrap/blob/master/.editorconfig) for easy use in common text editors. Read more and download plugins at <http://editorconfig.org>.



## Community

Keep track of development and community news.

- Follow [@twbootstrap on Twitter](http://twitter.com/twbootstrap).
- Read and subscribe to [The Official Bootstrap Blog](http://blog.getbootstrap.com).
- Chat with fellow Bootstrappers in IRC. On the `irc.freenode.net` server, in the `##twitter-bootstrap` channel.
- Implementation help may be found at Stack Overflow (tagged [`twitter-bootstrap-3`](http://stackoverflow.com/questions/tagged/twitter-bootstrap-3)).



## Versioning

For transparency into our release cycle and in striving to maintain backward compatibility, Bootstrap is maintained under [the Semantic Versioning guidelines](http://semver.org/). Sometimes we screw up, but we'll adhere to those rules whenever possible.



## Creators

**Mark Otto**

- <http://twitter.com/mdo>
- <http://github.com/mdo>

**Jacob Thornton**

- <http://twitter.com/fat>
- <http://github.com/fat>



## Copyright and license

Code and documentation copyright 2011-2014 Twitter, Inc. Code released under [the MIT license](LICENSE). Docs released under [Creative Commons](docs/LICENSE).
