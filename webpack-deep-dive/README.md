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

1. Add scripts to `package.json`
~~~~
  "scripts": {
     "build:dev": "webpack"
  }
~~~~

* To exclude npm errors issue run command with `-s` argument

~~~~
npm run build:dev
~~~~
2. Add `webpack.config.js`
3. Update `index.html`


## Specifying an entry point

## webpack-validator

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
