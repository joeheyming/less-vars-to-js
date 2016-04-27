[![Build Status](https://img.shields.io/travis/mjt01/less-vars-to-js/master.svg?style=flat-square)](https://travis-ci.org/mjt01/less-vars-to-js)
[![Coverage Status](https://img.shields.io/coveralls/mjt01/less-vars-to-js.svg?style=flat-square)](https://coveralls.io/github/mjt01/less-vars-to-js?branch=master)
[![npm](https://img.shields.io/npm/v/less-vars-to-js.svg?style=flat-square)](https://www.npmjs.com/package/less-vars-to-js)
# less-vars-to-js
Read [LESS](http://lesscss.org/) variables from the contents of a file and return them as a javascript object.
```js
$ npm install --save less-vars-to-js
```

### Why?
I wrote this to use in our living style guide where we document our colour palette, typography, grid, components etc. This allows variables to be visualised in the style guide without having to read through the code (useful for non-technical team members).

### What does it do?
Takes in the contents of a less file as a `string` and returns an `object` containing all the variables it found. It is deliberately naive as it is not intending to be a less parser. Basically it reads anything starting with an `@`, so it will ignore comments, rule definitions, import statements etc.

Example :
```less
@import (reference) "theme";

// Colour palette
@blue: #0d3880;
@pink: #e60278;

// Elements
@background: @gray;
@favourite: blanchedalmond;

// Grid
@row-height: 9;
```
Example output:
```js
{
  '@blue': '#0d3880',
  '@pink': '#e60278',
  '@background': '@gray',
  '@favourite': 'blanchedalmond',
  '@row-height': 9
}
```

### Usage
```js
import lessToJs from 'less-vars-to-js';
import fs from 'fs';

// Read the less file in as string
const paletteLess = fs.readFileSync('palette.less', 'utf8');

// Pass in file contents
const palette = lessToJs(paletteLess);

// Use the variables (example React component)
export default class Palette extends Component {
  render() {
    return (
      <div>
      {
        Object.keys(palette)
          .forEach(colour => (
            <div style={{ backgroundColor: palette[colour] }}>
              <p>{ colour }<p>
              <p>{ palette[colour] }</p>
            </div>
          ))
      }
      </div>
    );
  }
}
```

### License

[MIT](http://mjt01.mit-license.org)
