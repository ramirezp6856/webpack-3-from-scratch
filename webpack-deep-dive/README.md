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

`webpack.config.babel.js`
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

`webpack.config.babel.js`
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

`webpack.config.babel.js`
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
`webpack.config.babel.js`
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

is the same as

`webpack.config.babel.js`
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            filename: 'bundle.js'
        }
    })
~~~~

The difference being, in the future were going to
use an argument that is passed into the arrow function.
It makes differentiating between prod and dev config easier.

## webpack dev server
Argument `watch` watches for changes.
`npm run build:dev -s -- --watch`

devDependency
`"webpack-dev-server": "2.1.0-beta.0"`

Added as a script
`"dev": "webpack-dev-server"`

So
`npm run dev -sn`

Instead of this we can run the webpack dev server. Which watches and serves up http.


## Path configuration
Add path key to specify where the bundle will end up. It needs to be an absolute path so we use the function resolve.

`webpack.config.babel.js`
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = () => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            path: resolve('dist'),
            filename: 'bundle.js'
        }
    })
}
~~~~

To know where the bundle will be served you need add a publicPath.
If you don't specify publicPath it will assume the bundle served from just `/`.

`webpack.config.babel.js`
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = () => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            path: resolve('dist'),
            filename: 'bundle.js',
            publicPath: '/dist/'
        }
    })
}
~~~~

After editing config file you have to rebuild and then redeploy dev server.

## Minifying and source maps
Add script `"build": "webpack -p",`
Then `npm run build` will generate an optimized bundle.js in your dist.

**Source Maps** allow us to debug efficiently.

Adding `devtool: 'eval'` gives the browser the opportunity to debug webpack via source maps.
`eval` is really fast to generate however you don't want to ship that to customers.
By adding `devtool: 'source-map'`, The only thing that will be shipped to the customer will be our actual code and the url to our code.
See the difference below.

Then run 'npm run dev'.

`webpack.config.babel.js` for dev
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = () => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            path: resolve('dist'),
            filename: 'bundle.js',
            publicPath: '/dist/'
        },
        devtool: 'eval',
    })
}
~~~~

`webpack.config.babel.js` for prod
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = () => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            path: resolve('dist'),
            filename: 'bundle.js',
            publicPath: '/dist/'
        },
        devtool: 'source-map',
    })
}
~~~~

To access using chrome, inspect then click the Sources tab.
Under sources collapse localhost to bring `webpack://` into view.



## Development vs. production environments
We want to be able to distinguish between production and development in our configuration when
we're generating our configuration, and that's why we're using a function in our configuration
rather than an object, is because we can actually accept a parameter here.

The parameter is called `env`

`webpack.config.babel.js`
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = env => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            path: resolve('dist'),
            filename: 'bundle.js',
            publicPath: '/dist/'
        },
        devtool: 'source-map',
    })
}
~~~~

In our package JSON, we can set, in these scripts
we can set that env object to have a couple properties.
See below in the `scripts` section in terms of
`"dev": "webpack-dev-server --env.dev",` and `"build": "webpack -p --env.prod",.`

`package.json`
~~~~
{
  "private": true,
  "dependencies": {
    "todomvc-app-css": "2.0.6"
  },
  "devDependencies": {
    "eslint": "3.2.2",
    "eslint-config-kentcdodds": "^9.0.0",
    "ghooks": "1.3.2",
    "http-server": "0.9.0",
    "opt-cli": "1.5.1",
    "webpack": "2.1.0-beta.20",
    "webpack-validator": "2.2.7",
    "webpack-dev-server": "2.1.0-beta.0"
  },
  "config": {
    "ghooks": {
      "pre-commit": "opt --in pre-commit --exec \"npm run validate\""
    }
  },
  "scripts": {
    "build:dev": "webpack",
    "dev": "webpack-dev-server --env.dev",
    "build": "webpack -p --env.prod",
    "validate": "npm run lint",
    "start": "http-server --silent -c-1",
    "lint": "eslint ."
  }
}

~~~~

And then, inside of the webpack config file, we can say
env.prod and it will execute a ternary operator. See below
in `devtool` of the `module.exports`.

`webpack.config.babel.js`
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')


module.exports = env => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            path: resolve('dist'),
            filename: 'bundle.js',
            publicPath: '/dist/'
        },
        devtool: env.prod ? 'source-map' : 'eval',
    })
}
~~~~


## Exercise: Adding webpack

Restart and checkout the following branch

`git checkout -f FEM/01.0-add-webpack`

Some of the things that changed include webpack config's 'devTool' now uses a function as opposed to the itinerary statement.
Also, `package.json` was updated to include `prebuild` and `prebuild:dev` both utilize the package `rimraf`.

`webpack.config.babel.js`
~~~~
const webpackValidator = require('webpack-validator')
const {resolve} = require('path')
const {getIfUtils} = require('webpack-config-utils')


module.exports = env => {
    return webpackValidator({
        context: resolve('src'),
        entry: './bootstrap.js',
        output: {
            path: resolve('dist'),
            filename: 'bundle.js',
            publicPath: '/dist/'
        },
        devtool: ifProd('source-map' : 'eval'),
    })
}
~~~~

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
