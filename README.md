# React Leanjs Bolierplate

Create React apps with an Awesome Template.

This boilerplate should work on macOS, Windows, and Linux.<br>

## Quick Overview
### Getting Started
```sh
yarn run start
```

Then open [http://localhost:3000/](http://localhost:3000/) to see your app.

### Single Bundle Build
```sh
yarn run build
```
That will generate a single bundle with their minified version of Javascript and CSS. 

### Vendor + App Bundle Build
* First tou need to put all libraries used in the project, at the vendor file:
```
react-leanjs-bolierplate/
  vendor/
    vendor.js
```
As an example:
```
require('react')
require('prop-types')
require('react-dom')
```
* Then generate the vendor bundle minified, running the script:
```sh
yarn run build:dll
```
That will generate the vendor bundle minified and a manifest file used by webpack as a reference.<br>
Once this file is generated, if we do not add more libraries, we don't need to generated anymore.<br>

* Finally we can generate the App bundle:
```sh
yarn run build:split
```
That will generate only the App bundle with their minified version of Javascript and CSS.<br>

## Webpack 2 
### Features
* Adding [Polyfills.js] file. That will help to add transparency and not mention anymore ["whatwg-fetch"], ["object-assign"] and ["es6-promise"] in the development files files.<br>
All these files, plus some babel transformations (see babel chapter) it keeps React running in 95% of the browsers since 2011 (>=IE9). 
* Hot Reload & watcher of any type of file inside "src" folder.
* Error overlay in browser. No need to look more the console. 
* Check Linting before build.
* Omit files: ".test.(js|jsx)" and ".story.(js|jsx)".
* Add CSS transformations to support all browsers. ([postcss-loader]) Example:
```
Writing: 
box-shadow: 10px 10px 5px 0px rgba(0,0,0,0.75);

Will generate:
-webkit-box-shadow: 10px 10px 5px 0px rgba(0,0,0,0.75);
-moz-box-shadow: 10px 10px 5px 0px rgba(0,0,0,0.75);
box-shadow: 10px 10px 5px 0px rgba(0,0,0,0.75);
```
* [InterpolateHtmlPlugin] with [HtmlWebpackPlugin] Generates an `index.html` file with the <script> injected. No needed to serve with express server.
* [ManifestPlugin] Generates a manifest.json for the site.

### Performance
Added different tunne ups from React-create-app, Medium, Stack Overflow and more:
* Faster Development Build with "cheap-module-source-map" and cache of the previous build. Only will build the difference.
* Minified (JS, CSS, HTML), Gziped (JS, CSS), removed comments, deleted dead code, deleted unused, AggressiveMergingPlugin.
* [DllPlugin] & [DllReferencePlugin]: Helps to split in at least 2 files for lazy load or http2 load. The sum of the size of these 2 files is slightly bigger that the single one generated.
* [ExtractTextPlugin] Concentrate all CSS files into one external file. Is usefull to develop the css by components but is performance to load only one file.
* [StatsPlugin] Generates a file "profile.json" that we can upload to check our tree dependecies. We can upload [here](http://webpack.github.io/analyse/)

## Testing
### Jest, Enzyme & Enzyme-to-Json
Every file in "src" folder with the extension ".test.(js|jsx)" will be taken in the runner.
To run the test:
```sh
yarn run test
```

To run the coverage:
```sh
yarn run coverage
```
Open later the browser with the html generated under the folder:
```
react-leanjs-boilerplate/
  coverage/
    lcov-report/
      index.html
```

## Linting JS & CSS
We have included 3 types of linter with auto correction in most of the cases. Supported JS, JSX, CSS and SCSS.
It follow the rules of "Airbnb" and "Standard". They will be executed before any commit (Explained in "precommit")

### ESlint
It helps to detect and correct style issues in JS & JSX files. Can be manually executed:
```sh
yarn run eslint
```

### Stylefmt
It helps to detect and correct style issues in CSS & SCSS files. Can be manually executed:
```sh
yarn run stylefmt
```

### Prettier
It helps to correct style writing issues in JS & JSX files.Can be manually executed:
```sh
yarn run prettier
```

## React Story Book
React Storybook is something you can use with your app right away. <br>It will help to develop your UI components separately and track later as a documentation.

It has many features including:
* Completely isolate the environment for your components (with the use of various iframe tactics).
* HMR — hot module replacement (even for functional stateless components).
* Works with any React app (whether it’s Redux, Relay or any React app).
* Support for CSS (whether it’s plain old CSS, CSS modules or something fancy).
* Clean and fast user interface.
* Runs inside your project (so, it uses your app’s NPM modules and babel configurations out of the box).
* Serves static files (if you host static files inside your app).
* Extendable as required (support for custom webpack loaders and plugins).

That version uses a custom Config and Webpack (Version 1, still waiting the oficial update).
The plugins added to supercharge of powers that React Story Book are:

* [knobs addon](https://www.npmjs.com/package/@kadira/storybook-addon-knobs) It helps to add variables easy configure/play in real time.<br>
We can do Text, Number, Select, Date, Color, Array.... Best look playground [here](https://storybooks.js.org/storybook-addon-knobs/?knob-DOB=-2617594200000&knob-Name=Tom%20Cary&knob-today=1479515330701&knob-Color=black&knob-Passions%5B0%5D=Fishing&knob-Passions%5B1%5D=Skiing&knob-Favorite%20Number=42&knob-Style=%7B%22fontFamily%22%3A%22Arial%22%2C%22padding%22%3A20%7D&knob-My%20DOB=741983400000&knob-Bold=false&knob-Label=Hello%20Button&knob-Comfort%20Temp=72&selectedKind=Example%20of%20Knobs&selectedStory=with%20all%20knobs&full=0&down=1&left=1&panelRight=1&downPanel=kadirahq%2Fstorybook-addon-knobs)
Example:
```js
import React from 'react'
import { storiesOf, action } from '@kadira/storybook'
import { withKnobs, text, boolean, number, color, select } from '@kadira/storybook-addon-knobs'

storiesOf('Demo Knobs', module)
  .addDecorator(withKnobs)
  .add('as dynamic variables', () => {
    const name = text('Name', 'Arunoda Susiripala')
    const options = {
      range: true,
      min: 60,
      max: 90,
      step: 1,
    }
    const age = number('Age', 73, options)

    const defaultValue2 = '#ff00ff'
    const textColor = color('Color', defaultValue2)

    const options2 = {
      red: 'Red',
      blue: 'Blue',
      yellow: 'Yellow',
    }
    const backColor = select('Background Color', options2, 'red')

    const content = `I am ${name} and I'm ${age} years old.`
    return (<div style={{ color: textColor, backgroundColor: backColor }}>{content}</div>)
  })
```

* [storybook-host](https://www.npmjs.com/package/storybook-host) decorator with powerful display options for hosting, sizing and framing your components.
Example:
```js
import React from 'react'
import { storiesOf, action } from '@kadira/storybook'
import { host } from 'storybook-host'
import Spinner from './Spinner' //--> This will be your component to test

storiesOf('Spinner Loader', module)
  .addDecorator(host({
    title: 'A host container for components under test.',
    align: 'center bottom',
    height: '80%',
    width: 400,
  }))
  
  ...
  
  .add('Without props', () => (
    <Spinner />
  ))
```

* [react-storybook-addon-intl](https://www.npmjs.com/package/react-storybook-addon-intl) It helps you to test yout "react-intl" components in the differents locales.
Example:
```js
...

import { addLocaleData } from 'react-intl'
import ru from 'react-intl/locale-data/ru'
import es from 'react-intl/locale-data/es'
import de from 'react-intl/locale-data/de'
addLocaleData(ru)
addLocaleData(es)
addLocaleData(de)

storiesOf('Buttons', module)
  .addWithIntl(
    'Initial Locale (de-DE)',
    () => (<Button action={action('another test')} />),
    {
      'ru-RU': {
        'button.caption': 'Нажми меня! (ru-RU)'
      },
      'de-DE': {
        'button.caption': 'Klick mich (de-DE)'
      },
      'es-ES': {
        'button.caption': 'Haz click en mi (es-ES)'
      }
    },
    {
      initialLocale: 'de-DE'
    }
  )
```

* [@kadira/react-storybook-addon-info](https://www.npmjs.com/package/@kadira/react-storybook-addon-info) It helps to write infor of the component proptypes as a generic documentation. <br>
We can generate Static files later to keep track of styling in the project.
Example:
```js
...
storiesOf('Buttons', module)
  .addWithInfo(
    'simple usage (inline info)',
    `
      This is the basic usage with the button with providing a label to show the text.
    `,
    () => <Button label="The Button" onClick={action('onClick')} />,
    { inline: true },
  )
```

* [react-storybook-addon-props-combinations](https://www.npmjs.com/package/react-storybook-addon-props-combinations) It helps to write in a sintetic way all differents propTypes that will change the component,<br>
in order to test and view the result in a maintainable way. Every posibility has to be written inside of an array.
Example:
```js
...

storiesOf('Buttons', module)
  .addWithPropsCombinations(
    'Standard usage',
    Button,
    {
      disabled: [false, true],
      onClick: [action('clicked')],
      label: ['hello world', <b>some elements</b>]
    },
    {
      CombinationRenderer: ({Component, props, options}) => (
        <Component {...props} />
      )
    }
  )

```

* [storybook-addon-material-ui](https://github.com/sm-react/storybook-addon-material-ui) That is awesome, it helps you to tune, play and update you components with Material UI and their themes or custom theme.
Example:
```js
...
import {muiTheme} from 'storybook-addon-material-ui'

const newTheme = {
  themeName: 'Grey Theme',
  palette: {
    primary1Color: '#00bcd4',
    alternateTextColor: '#4a4a4a',
    canvasColor: '#616161',
    textColor: '#bdbdbd',
    secondaryTextColor: 'rgba(255, 255, 255, 0.54)',
    disabledColor: '#757575',
    accent1Color: '#607d8b',
  },
}

storiesOf('Material-UI', module)
  .addDecorator(muiTheme([newTheme]))
  .add('Button Example Controlled', () => (
    <Button label="The Button" />
  ))

```

And when we download our custom theme, we can added, like this:
```js
...

import greyTheme from './greyTheme.json'

storiesOf('Material-UI', module)
  .addDecorator(greyTheme)
  .add('Button Example Controlled', () => (
    <Button label="The Button" />
  ))

```

### Run React Story Book
We only need to run the next command and browser on [http://localhost:9001/](http://localhost:9001/) to play around.
```sh
yarn run storybook
```

### Add stories 
It will only look inside "src" folder any file with the extension "*.story.(js|jsx)".
Example of simple file, that will generate one of the menus, very "add" will be a case to show:
```js
import React from 'react'
import { storiesOf, action } from '@kadira/storybook'
import Spinner from './Spinner' //--> This will be your component to test

storiesOf('Spinner Loader', module)
  .add('Without props', () => (
    <Spinner />
  ))
```

## Flow
Static type checker, ultra recommended. We only need to create a comment on top:
```
/* @flow */
```
To take the power of Flow we need to spend time learning and creating the full checks, not a simple object or function check. That will not help at all.
Recommend to check some of the [builtin types](https://github.com/facebook/flow/blob/master/lib/dom.js) usefull for Event.target.value and so on.
Example:
```js
/* @flow */
type QuantityButtonsProps = {
  quantity: number,
  addCourse: () => void,
  remCourse: () => void,
  inputCourse: (Event) => void
}

const QuantityButtons = ({ quantity, addCourse, remCourse, inputCourse }: QuantityButtonsProps) => ( ... )

```

## Babel
We keep our config very updated for performance and support to latest features.
* Babel lastest with support for old browsers (>=IE9 - 2011).
* Stage-0 Experimental to use all new features.
* Flow support.
* Added some transformations for performance as :
  * "babel-plugin-transform-class-properties".
  * "babel-plugin-transform-object-rest-spread".
  * "babel-plugin-transform-react-jsx".
  * "babel-plugin-transform-regenerator" Transforms Async to Generators.
  * "babel-plugin-syntax-dynamic-import".
  * "transform-react-inline-elements".
  * "transform-react-constant-elements".

## Pre-commit
Hook executed when we create a commit. That will execute the 3 linters (ESlint, Stylefmt and Prettier) over the files added. <br>
If it doesn't pass the reformat, the commit won't be generated.

## Ready for React 15.5
* PropTypes is outside React.
```js
import PropTypes from 'prop-types'

const QuantityButtons = ({ quantity, addCourse, remCourse, inputCourse }) => ( ... )

QuantityButtons.propTypes = {
  quantity: PropTypes.number,
  addCourse: PropTypes.func,
  remCourse: PropTypes.func,
  inputCourse: PropTypes.func
}

QuantityButtons.defaultProps = {
  quantity: 1,
  addCourse: () => {},
  remCourse: () => {},
  inputCourse: () => {}
}
```

We will keep updated. That is our default stack, it helps to develop faster. <br>
At the moment we don't need to add Redux, React Router and extras, as they are not used it in every project (Small or Big).
