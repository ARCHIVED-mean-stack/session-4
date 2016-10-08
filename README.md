#MEAN Session Four

`$cd` to sushi and run `$ sudo npm install`.

Use the default gulp task to run the page in the browser (re-examine the gulpfile). Note that Browser Sync offers a UI:

```
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

Try using an FTP client and then use SSH.

`$ ssh pezuaj@oit2.scps.nyu.edu`

Note: you can use `$git clone ... ` at this point if you wish.

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
    remotePath: '/home/p/pezuaj/web'
  }));
});
```



##Homework

1. add gulp-sftp to your package and gulpfile and publish your homework to the server. 


##Reading

Dickey - Write Modern Web Apps with the MEAN Stack: Mongo, Express, AngularJS and Node.js, chapters 1-2. His [Github repo with the book code](https://github.com/dickeyxxx/mean-sample)

[Mozilla on DOM Scripting](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

NOTES

https://github.com/DannyBoyNYC/session-3-dd/tree/gulping-scripts/scripting

http://daniel.deverell.com/mean-fall-2016/scripting-angular-sample.zip 

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


window.onload = function() {
    addContent();
    prepareGallery();
}

function addContent(){
    var gallery = document.getElementById("imageGallery");
    var links = gallery.getElementsByTagName("a");
    for ( var i=0; i < links.length; i++ ) {
       links[i].setAttribute('title', myObject.entries[i].title);
       links[i].setAttribute('href', 'img/' + myObject.entries[i].picture[0]);
       links[i].firstChild.nodeValue = myObject.entries[i].name;
   }
};



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



