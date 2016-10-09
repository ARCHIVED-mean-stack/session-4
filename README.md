#MEAN Session Four

`$cd` to sushi and run `$ sudo npm install`.

Use the default gulp task to run the page in the browser (re-examine the gulpfile). Note that Browser Sync offers a UI:

```sh
 ------------------------------------
       Local: http://localhost:3000
    External: http://192.168.1.8:3000
 ------------------------------------
          UI: http://localhost:3001
 UI External: http://192.168.1.8:3001
 ------------------------------------
```



##Sushi - Mobile First Navbar

Let's assume [this](http://daniel.deverell.com/mean-fall-2016/gulp/) is our goal. 

Review the CSS Tricks page on Flex settings.

In nav.scss remove `max-width: 140px;` and add:

```css
li {
  flex: 1 1 auto;
}
```

Note that, as it stands, our nav CSS is not mobile first. Move the span contents into the media query.

Move the span contents into the media query.

```css
span {
  display: none;
}
```

Make display adjustments to the nav bar (padding, icon size) and refactor the media queries so that they are distributed within thier respective selectors. 

Remove the jQuery material from both pages.

Here is the final navigation sass:

```css
.main-nav {
  background: #eee;
  margin-bottom: 1em;

  ul {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  li {
    flex: 1 1 auto;
  }
  a {
    padding: 0.5rem 0.25rem;
    font-size: 1rem;
    font-weight: bold;
    display: flex;
    color: $reddish;
    background-color: $tan;
    &:hover, &:focus {
      background-color: $reddish;
      color: $white;
      svg {
        fill: $white; 
      }
      span {
        color: $white; 
      }
    }
    @media (min-width: $break-one) {
      font-size: 1.5rem;
      padding: 1.25rem 0.5rem;
    }
  }
  span {
    display: none;
    @media (min-width: $break-one) {
      display: block;
      font-size: 0.875rem;
      font-weight: normal;
      color: #888;
      margin: 0.25rem 0 0 0;
    }
  }
  .icon {
    width: 18px;
    height: 18px;
    float: left;
    margin-right: 0.5rem;
    fill: #999;
    @media (min-width: $break-one) {
      width: 40px;
      height: 40px;
      margin-right: 1rem;
    }
  }
}
```



##Server Accounts

* Username is the first seven letters of your last name + first letter of first name
* Password is first initial, last initial, 123890
* e.g. devereld // dd123890
* Hostname is oit2.scps.nyu.edu

Test to see if your account is active by entering this URL into a new browser tab (use your username after the tilde):
```
http://oit2.scps.nyu.edu/~pezuaj/
```

Try using an FTP client and then use SSH and cd into the web directory:

`$ ssh pezuaj@oit2.scps.nyu.edu`

Only the web directory has been configured to host external http. Note: you can use `$git clone <path to git file on github> ` in this folder to deploy a project. Here we will be deploying directly from your working directory on your computer.

`$ pwd`

`$ sudo npm install --save-dev gulp-sftp`

```js
var sftp = require('gulp-sftp');

gulp.task('deploy', function() {
  return gulp.src('./app/**/*')
  .pipe(sftp({
    host: 'oit2.scps.nyu.edu',
    user: '****',
    pass: '****',
    remotePath: '/home/p/<user>/web'
  }));
});
```

##NODE

A simple node.js [server](https://nodejs.org/en/about/).

##Express

Let's look at the canonical "Hello world" [example](https://expressjs.com/en/starter/hello-world.html). 

Here is the [generator](https://expressjs.com/en/starter/generator.html). Note the directory structure and the use of Jade as a template tool.

We will be using HTML [static](https://expressjs.com/en/starter/static-files.html) files in our exercise.

Examine the `package.json` and `app.js` files in `scripting` - a generic Express app pointing to our public folder for static assets.

Note that the js file is now separate and linked at the bottom of our index page.

###Setup

Before we edit this page let's implement our workflow.

Add gulp, gulp-sass, gulp-sourcemaps and browser-sync to the list of devDependencies:

```js
{
  "name": "pictureviewer",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Daniel Deverell",
  "license": "MIT",
  "dependencies": {
    "express": "^4.14.0"
  },
  "devDependencies": {
    "browser-sync": "^2.16.0",
    "gulp": "^3.9.1",
    "gulp-sass": "^2.3.2",
    "gulp-sourcemaps": "^1.6.0"
  }
}

```

Since gulp is just JavaScript we can forgo the use of a gulpfile and continue to use our app.js file for both gulp and express. Note the use of [proxy and a specific browser](https://www.browsersync.io/docs/options/#option-browser) for browser sync:

```js
var gulp = require('gulp');
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');
var browserSync = require('browser-sync')
var express = require('express');

var sassOptions = {
  errLogToConsole: true,
  outputStyle: 'expanded'
};

var sassSources = './app/public/sass/**/*.scss';
var sassOutput = './app/public/css';
var htmlSource = './app/public/**/*.html';

var app = express();
var port = process.env.PORT || 3000;

gulp.task('sass', function(){
  return gulp.src(sassSources)
  .pipe(sourcemaps.init())
  .pipe(sass(sassOptions).on('error', sass.logError))
  .pipe(sourcemaps.write('.'))
  .pipe(gulp.dest(sassOutput))
  .pipe(browserSync.stream())
});

function listening () {
  browserSync({
    proxy: 'localhost:' + port,
    browser: "google chrome"
  });
    gulp.watch(sassSources, ['sass']);
    gulp.watch(htmlSource).on('change', browserSync.reload);
}


app.use(express.static('./app/public'));

app.listen(port, listening);
```

Run `$ sudo npm install --save-dev <library>`

Run `$ node app.js` and test to ensure that both sass and html changes refresh the browser and that a css .map file is created. We are not watching the js directory yet so we still have to do some manual refreshing.

###Adjust Formatting

Since we are working mobile first let's set up the display in a narrow browser.

```css
body {
  font: 1em/1.5 Helvetica, arial, sans-serif;
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.active img {
  border-color: #666;
}

img {
  width: 100%;
  border: 4px solid #bbb;
}
```
Use [flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) for the image gallery

```css
#imageGallery {
  list-style: none;
  display: flex;
}

#imageGallery li {
  flex: 1 1 auto;
}
```
Position the caption so that it overlays the main image:

```css
#content {
  position: relative;
}
#content img {
  border: none;
}
#content p {
  background-color: rgba(0,0,0,0.5);
  position: absolute;
  bottom: 0.5rem;
  color: white;
  padding: 1rem;
  width: 100%;
}
```

##DOM Scripting 

Let's refactor our js to use `classList`:

```js
...
links[0].parentNode.classList.add('active');
...
links[i].parentNode.classList.remove('active');
...
whichPic.parentNode.classList.add('active');
```

This is a new feature in HTML5 and an example of how advances in js are making jQuery [less and less relevant for web designers](https://medium.freecodecamp.com/how-to-manipulate-classes-using-the-classlist-api-f876e2f58236#.bmo0nynrj).

Add an Object to our js file:

```js
var myObject = {
  "entries":[
{
  "title": "Yellow pagoda by a river.",
  "name": "Yellow Pagoda",
  "picture": ["pagoda.jpg"]
},
{
  "title": "Red bridge over the river.",
  "name": "Red Bridge",
  "picture": ["bridge.jpg"]
},
{
  "title": "Green bamboo is the material.",
  "name": "Green Bamboo",
  "picture": ["bamboo.jpg"]
},
{
  "title": "Red stairway to the temple.",
  "name": "Red Stairway",
  "picture": ["stairway.jpg"]
}
]
};
```

We will use this object to look at the connection between data (the "model") and the html (the "view").

In the browser's console 
* `typeof myObject`
* `myObject`
* `myObject.entries`
* `myObject.entries.length`
* `myObject.entries[0]`
* `myObject.entries[0].picture`
* `myObject.entries[0].picture[0]`

To use this object let's add a new function - `addContent()`:

```js
window.onload = function(){
  addContent();
  prepareGallery();
};
```

Populate the html using data from the object:

```js
function addContent(){
    var gallery = document.getElementById("imageGallery");
    var links = gallery.getElementsByTagName("a");
    for ( var i=0; i < links.length; i++ ) {
        links[i].setAttribute('title', myObject.entries[i].title);
        links[i].setAttribute('href', 'img/' + myObject.entries[i].picture[0]);
        links[i].firstChild.nodeValue = myObject.entries[i].name;
  }
};
```

Add a new node to the end of the html file:

```html
<div id="test"></div>
```

Dynamically create a new navigation list

```js
function addContent(){
    var newgallery = document.createElement('h2')
    var newContent = document.createTextNode("Dynamic Gallery")
    newgallery.appendChild(newContent)
    var currentLoc = document.getElementById('test')
    document.body.insertBefore(newgallery, currentLoc)
    var newList = document.createElement('ul')
    document.body.appendChild(newList)
    for (var i=0; i < myObject.entries.length; i++){
        var li = document.createElement("li")
        var a = document.createElement("a")
        a.innerHTML = myObject.entries[i].name
        a.setAttribute('href', 'img/' + myObject.entries[i].picture[0])
        a.setAttribute('title', myObject.entries[i].title)
        li.appendChild(a)
        newList.appendChild(li)
    };
    document.body.insertBefore(newList, currentLoc); 
};

```

The amount of work required to develop the page dynamically is one of the reasons frameworks such as Angular have become popular.

##Angularizing our Gallery

```html
<!DOCTYPE html>
<html>
<head>
  <title>Image Gallery</title>
<script src="https://code.angularjs.org/1.2.3/angular.js"></script>
<script src="js/alt.js" charset="utf-8"></script>
    <link rel="stylesheet" href="css/styles.css">
</head>

<body ng-app>
    <h1>Image Gallery</h1>
    <div ng-controller="ListController">
        <ul id="imageGallery">
            <li ng-repeat="entry in entries">
                <a href="img/{{entry.picture[0]}}" title="{{entry.title}}">{{ entry.name }}</a>
            </li>
        </ul>
    </div>
    <div id="content">
      <img id="placeholder" src="img/placeholder.gif" alt="Placeholder">
      <p id="description">Select an image.</p>
    </div>
</body>
</html>
```


##Homework

1. add gulp-sftp to your homework package.json file, edit your gulpfile and publish your homework to a server. 


##Reading

Dickey - Write Modern Web Apps with the MEAN Stack: Mongo, Express, AngularJS and Node.js, chapter 3. Please attempt to implement his sample app on your computer. Here's his [Github repo with sample code](https://github.com/dickeyxxx/mean-sample)

[Mozilla on DOM Scripting](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

NOTES

http://book.mixu.net/node/ch5.html


