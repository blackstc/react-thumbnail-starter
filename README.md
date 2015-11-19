## Background
In this tutorial, we are going to refactor the bootstrap thumbnail component into React components.  Here is a link the completed [github repo](https://github.com/blackstc/react-thumbnail-homework).  I would recommend doing the tutorial first and visiting this at the end if you have any problems.  

## Wire Frame
Here is a basic picture of what we want our end result to be.  We are going to make two thumbnails that show the type of tutorial, an image, and a button with a number of tutorials for that related subject.  From the picture, can you guess how many React components there are? ![wireframe](./img/overview.png)

## Setup
First, you may want to install a React package for your text-editor before we get started.  This will help with syntax highlighting when we start writing JSX files.  Second, you can fork and clone this repository.  I've created a new branch for each section as we move along the course if you get lost.

After you have cloned this repository, run:

```sh
$ npm i
```

### Gulp
React components are written in JSX.  JSX gives you the ability to write HTML within javascript files.  This is the life-blood of building React components, however, the JSX files need to be compiled into javascript. The easiest way to do this is with the gulp build process.  This gulp file will not only compile from JSX to javascript, but will order the files properly and send all the code to a single file, main.js.

Run gulp from the console to start your compiler.  Leave the gulp file running and it will automatically update with your main.js whenever changes are made.


The gulp file will also run a browser sync to that it will automatically update the browswer whenever we make changes.

### Step One
The app jsx is the the platform to launch your JSX files.  For now, we will use it to attach elements to the DOM and as a place to old our sample data.

First we need to require in React and additional components that we want to render. Secondly we will require a thumbnail-list, this is a component we will build later.
```javascript
var React = require('react');
var ThumbnailList = require('./thumbnail-list');
```

Next let's add the sample data that we will use for our thumbnails.
```javascript
var options = {
  thumbnailData: [
    {
      title: "See tutorials",
      number: 32,
      header: 'Learn React',
      description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.',
      imageUrl: 'http://formatjs.io/img/react.svg'
    },
    {
      title: "See tutorials",
      number: 12,
      header: 'Learn Gulp',
      description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.',
      imageUrl: 'https://raw.githubusercontent.com/gulpjs/artwork/master/gulp-2x.png'
    }
  ]
};
```

Finally, we will add some react to create our components and append them to the DOM.
```javascript
// React, please render this class
var element = React.createElement(ThumbnailList, options);

// React, after you render this class, please place it in my body tag
React.render(element, document.querySelector('.target'));
```

### Badge

When looking at our thumbnails the first component we are going to make is the Badge.  The badge will act as a button on our thumbnail that will have text and show the number of tutorials for that specific subject.  So lets get to it...

First, we will setup, what will be our basic boilerplate for starting every component in React.

```javascript
var React = require('react');

module.exports = React.createClass({
  render: function() {
  }
});
```
You will notice that the entire component will be wrapped in a 'module.exports'.  This will make it much easier to import this file later, and then simply paste the component where it is needed.  Don't worry if this is confusing, we will go into more detail about this throughout the tutorial.

Next, lets add our code to the render function.
```javascript
var React = require('react');

module.exports = React.createClass({
  render: function() {
    return <button className="btn btn-primary" type="button">
    {this.props.title} <span className="badge">{this.props.number}</span>
    </button>
  }
});
```
Now, we have seen our first example of JSX and passing through information into a component.  Looking at this completed component, three things stand out:

1. className
1. {this.props.title}
1. {this.props.number}
  1. When writing JSX, we call React methods by wrapping them in {}
  1. React components use properties on their JSX elements to pass data through to each component.  Components can then call that data using 'this.props'.  We'll see more of this in our next component.


## Thumbnail
Now lets create a single thumbnail component using the bootstrap thumbnail html as our foundation.  Again, first lets start with our basic boilerplate; however, this time we will also require in our Badge component that we just created so that we can use it in our thumbnail componenet;

```javascript
var React = require('react');
var Badge = require('./badge');

module.exports = React.createClass({
  render: function() {
  }
});
```

Now we'll add the basic bootstrap outline for creating a thumbnail component.

```javascript
var React = require('react');
var Badge = require('./badge');

module.exports = React.createClass({
  render: function() {
    return <div class="col-sm-6 col-md-4">
      <div class="thumbnail">
        <img src="..." alt="...">
        <div class="caption">
          <h3>Thumbnail label</h3>
          <p>...</p>
          <p><a href="#" class="btn btn-primary" role="button">Button</a> <a href="#" class="btn btn-default" role="button">Button</a></p>
        </div>
      </div>
    </div>
  }
});
```

Similar to the Badge component, lets first go through and add our {this.props} to items we want to by dynamic.

```javascript
var React = require('react');
var Badge = require('./badge');

module.exports = React.createClass({
  render: function() {
    return <div className="col-sm-6 col-md-4">
      <div className="thumbnail">
        <img src={this.props.imageUrl} />
        <div className="caption">
          <h3>{this.props.header}</h3>
          <p>{this.props.description}</p>
          <p><a href="#" class="btn btn-primary" role="button">Button</a> <a href="#" class="btn btn-default" role="button">Button</a></p>
        </div>
      </div>
    </div>
  }
});
```

Now lets replace the button with our badge component that we just created.  Be sure to analyze how the Badge component is added.  How are we passing data through the Badge component?

```javascript
var React = require('react');
var Badge = require('./badge');

module.exports = React.createClass({
  render: function() {
    return <div className="col-sm-6 col-md-4">
      <div className="thumbnail">
        <img src={this.props.imageUrl} />
        <div className="caption">
          <h3>{this.props.header}</h3>
          <p>{this.props.description}</p>
          <p>
            <Badge title={this.props.title} number={this.props.number}/>
          </p>
        </div>
      </div>
    </div>
  }
});
```

## Thumbnail List

This final part will definitely be the most complicated component we have made thus far, but I will try to explain as best as possible, however, this will probably just take some time and repetition to fully understand what is happening.

```javascript
var React = require('React');
var Thumbnail = require('./thumbnail');

module.exports = React.createClass({
  render: function() {
    var list = this.props.thumbnailData.map(function(thumbnailProps) {
      return <Thumbnail {...thumbnailProps} />
    });

    return <div>
      {list}
    </div>
  }
});
```

Lets talk about what is weird here:

1. var list
  1. First, we'll see we are using some functional programming by calling map on 'thumbnailData'.  You'll notice that we pass thumbNail data into this component from App.jsx
  1. Second, we're using map because we are passing in data for two separate thumbnails, remember our wireframe from above? In this map function we want to return two separate thumbnail components.
  1. Third, within the Thumbnail component we see '{...thumbnailProps}', this is called spread attributes, and the '...' is called  a spread operator
    1. the spread operator actually exists in ES6 on arrays, and JSX is trying to take advantage of the syntax
    1. but this spread attribute  is used to pass through all the data in a more concise matter instead of passing them through, one attribute at a time.
1. Finally the return at the bottom is simply returning the list of Thumbnail components we just created in a list.

## Conclusion
So if we run:
```sh
$ gulp
```
And now we open our index.html, we should see our two thumbnail components on the page.
