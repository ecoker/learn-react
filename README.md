Props
-----

React works with two basic types of data. *Props* and *State*. When working with _Props_, I want you to think _immutable_.

Components get props. They _do not change_ props. A parent provides props to a child component and children always listen to their parents (right?).

Let's start with our `Menu` _Global Component_.

```
import React, { Component } from 'react'

class Menu extends Component {
  render() {
    var menuItems = this.props.menuItems.map(function(menuItem, i){
        return <li key={ `menu-item-${i}` }><a href={`#${ menuItem.location }`}>{ menuItem.text }</a></li>
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