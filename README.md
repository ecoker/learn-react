Props
-----

React works with two basic types of data. *Props* and *State*. When working with _Props_, I want you to think _immutable_.

Components get props. They _do not change_ props. A parent provides props to a child component and children always listen to their parents (right?).

Let's start with our `Menu` _Global Component_.

_src: `/assets/js/commponents/Menu.jsx`_
```
import React, { Component } from 'react'

class Menu extends Component {
  render() {
    var menuItems = this.props.menuItems.map(function(menuItem, i){
        return <li key={ `menu-item-${i}` }><a href={`${ menuItem.location }`}>{ menuItem.text }</a></li>
    })
    return (
        <header>
            <ul>
                { menuItems }
            </ul>
        </header>
    );
  }
}

Menu.defaultProps = {
    menuItems:[]
}

export default Menu
```

Lots of new stuff here. Let's break it down.

### Accessing Props
First things first, how are we accessing props? Props are accessed using `this.props`. They will come in as an object with key names that match the attribute name you use when passing variables into the component.

### Turning Props into Elements
This isn't always necessary, but I recommend using `map` to turn the array into an array of elements. To display on the page, but there's a catch!

Did you notice the `key` attribute? Every item in an array of elements needs a unique `key` value. This is what React uses to manipulate an element. Things can get weird if these values aren't unique.

### Default Props
Since this is used all over the place, sometimes you'll forget that you really needed to define something to keep it from breaking. :)

Using this ES6 version of the React syntax, you can define `defaultProps` on your _Component_ to make your application a bit more stable.

---------

## Passing Props to Child Components

Let's crack open that _Global Component_ and pass some props to our `Menu`

_src: `/assets/js/containers/Example/index.jsx`_
```
import React, { Component } from 'react'

import Menu from 'components/Menu.jsx'
import FancyTitle from 'components/FancyTitle.jsx'

class Example extends Component {
  render() {
    /* 
    // LATER THIS SHOULD COME FROM A SERVICE OR API CALL
    */
    var menuData = {
        menuItems: [
            {
                text: 'home',
                location: '/'
            },
            {
                text: 'about',
                location: '/#about'
            },
            {
                text: 'contact',
                location: '/#contact'
            }
        ]
    }
    return (
        <div>
            <Menu { ...menuData } />
            <FancyTitle />
        </div>
    )
  }
}

export default Example
```

Great! Now we're passing data into the menu. I want to call out something interesting here:
```
<Menu { ...menuData } />
```

We can use the spread operator to pass multiple props into a component. For example, this:
```
<Menu menuItems={ menuItems } title="Example" size="large" />
```

Is the same as taking this object:
```
var menuData = {
    menuItems:[],
    title: "Example",
    size: "large"
}
```

And setting the attributes like we did in our example:
```
<Menu { ...menuData } />
```

I was _very_ excited when I found that. I hope you enjoy it too.

### Back to business
Let's go ahead and update our `FancyTitle` _Component_ while we're in here.

_src: `/assets/js/commponents/FancyTitle.jsx`_
```
import React, { Component } from 'react'

class FancyTitle extends Component {
  render() {
    return (
        <h4 className="fancy-title">{ this.props.text }</h4>
    );
  }
}

FancyTitle.defaultProps = {
    text: "I need a text prop!"
}

export default FancyTitle
```

Let's pass in that `text` prop in our _Container Component_.

_src: `/assets/js/containers/Example/index.jsx`_
```
import React, { Component } from 'react'

import Menu from 'components/Menu.jsx'
import FancyTitle from 'components/FancyTitle.jsx'

class Example extends Component {
  render() {
    /* 
    // LATER THIS SHOULD COME FROM A SERVICE OR API CALL
    */
    var menuData = {
        menuItems: [
            {
                text: 'home',
                location: '/'
            },
            {
                text: 'about',
                location: '/#about'
            },
            {
                text: 'contact',
                location: '/#contact'
            }
        ]
    }
    return (
        <div>
            <Menu { ...menuData } />
            <FancyTitle text="I'm a Prop!" />
        </div>
    )
  }
}

export default Example
```

Try removing the text prop to see your `defaultProp` show up on the `FancyTitle` component. This is an example of using defaultProps as a simple form of documentation. Kinda hacky, but useful!

---------

## Time for a Bit of Clean up
We really don't want to be creating that array of elements inside the render function. I mean, we can, but it's a little ugly. Let's do it inside a function.

_src: `/assets/js/commponents/Menu.jsx`_
```
import React, { Component } from 'react'

class Menu extends Component {
  
    constructor(props){
        super(props)
        this.getMenuItems = this.getMenuItems.bind(this)
    }

    getMenuItems() {
        return this.props.menuItems.map(function(menuItem, i){
            return <li key={ `menu-item-${i}` }><a href={`${ menuItem.location }`}>{ menuItem.text }</a></li>
        })
    }

    render() {
        return (
            <header>
                <ul>
                    { this.getMenuItems() }
                </ul>
            </header>
        );
    }

}

Menu.defaultProps = {
    menuItems:[]
}

export default Menu
```

Here, we introduce a few more things. First off, props are available to the render function, but not to custom functions like `getMenuItems` by default. We need to bind the `this` value inside the contructor to expose `props` to the component.

```
constructor(props){
    super(props)
    this.getMenuItems = this.getMenuItems.bind(this)
}
```

After we do that, we can comfortably access what we need.

Here's a preview of what your page should look like now:
![Learn React Preview](walkthrough/images/props-preview.png "Props Preview")


_Bonus Points_: Create a new `MenuItem` component and use that for the menu items instead of just returning a list element. It won't be part of our repo going forward, but it may be fun to play with.

---------

Ready for more?
- [Basic Components](https://github.com/ecoker/learn-react/tree/basic-components)
- Props
- [State](https://github.com/ecoker/learn-react/tree/state)
- [Redux](https://github.com/ecoker/learn-react/tree/redux)
- [Forms](https://github.com/ecoker/learn-react/tree/forms)
- [Events](https://github.com/ecoker/learn-react/tree/events)
- [SVGs](https://github.com/ecoker/learn-react/tree/svgs)
- [Stylesheets](https://github.com/ecoker/learn-react/tree/stylesheets)