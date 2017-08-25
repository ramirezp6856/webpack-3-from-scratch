# Webpack Deep Dive
Kent Dodds Lynda

# Reference Project
`https://github.com/kentcdodds/es6-todomvc.git`

# 1. Getting Started with webpack

## Initializing webpack
~~~~
    "webpack": "2.1.0-beta.20"
~~~~

To exclude npm errors issue run command with `-s` argument
~~~~
npm run build:dev
~~~~

### Config Props:
~~~~
entry/output.filename
~~~~

#### Add scripts to `package.json`
~~~~
  "scripts": {
     "build:dev": "webpack"
  }
~~~~

**Note** To exclude npm errors issue run command with `-s` argument

~~~~
npm run build:dev -s
~~~~
#### Add `webpack.config.js`
~~~~
touch webpack.config.js
~~~~
A fun little trick is to add `.babel` between `.config` and `.js`.
Then you can transpile with babel.

Now we have to create a module inside of `webpack.config.js`.


## Specifying an entry point
Entry is like the main method of our app. It's responsible for requiring all the things it needs all its dependencies and
webpack will resolve dependencies and spit out the resulting bundle or the concatenated resolved file.

Provide webpack with context. All our modules are in the src directory.
The following will help you create the bundle.js file.

Webpack ships with a runtime. Webpack turns all your require statements and import statements into a
webpack require to resolve all the modules at runtime.

~~~~
const {resolve} = require('path')

module.exports = () => {
    return {
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            filename: 'bundle.js'
        }
    }
}
~~~~

#### Update `index.html`
Now that we have the bundling we can change reference from bootstrap.js to bundle.js.

## webpack-validator
Pass your webpack configure object to the webpack-validator function.
Webpack-validator will give you friendly errors. Remember the `-s` argument.

`npm run build:dev -s`

~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = () => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            filename: 'bundle.js'
        }
    })
}
~~~~

OR

~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = () => {
    {
        const config = {
            context: resolve('src'),
            entry: './bootstrap.js',
            output: {
                filename: 'bundle.js'
        }
    }

    return webpackValidator(config)
}
~~~~

#### `{resolve}`
`{resolve}` is an example of destructuring. Node 6 allows you to do this.
~~~~
const {resolve} = require('path')
~~~~

is the same as

~~~~
const path = require('path')
const resolve = path.resolve
~~~~

and the same as

~~~~
const resolve = require('path').resolve
~~~~

#### Arrow function


## webpack dev server

## Path configuration

## Minifying and source maps

## Development vs. production environments

## Excercise: Adding webpack

# 2. Working with webpack

## Debugging webpack

## Bundling

## Explicit dependencies

## Transpiling with Babel

## Loading CSS

## Excercise: Using the style and CSS loaders

## Hot module replacement

# 3. Testing with webpack

## Exercise: Initialize Karma

## Exercise: Using Karma with webpack

## Solution: Using Karma with webpack

## Coverage basics

## Exercise: Code coverage

## Coverage exclusions and webpack middleware

## Exercise: Coverage thresholds

# 4. webpack Optimizations

## Exercise: ES6ify

## Tree shaking

## Exercise: Adding tree shaking

## Code splitting

## Exercise: Using System.import()

## Solution: Using System.import()

## Commons chunking

## Exercise: Adding commons chunking, part 1

## Exercise: Adding commons chunking, part 2

## Exercise: Adding commons chunking, part 3

## Exercise: Long-term caching

## Exercise: Extracting CSS, part 1

## Exercise: Extracting CSS, part 2

## Offline with service workers

## Deploying to Surge.sh
