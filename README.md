# webpack-3-from-scratch

## Getting Started
`npm install`

## Creating a Bundle
Notice, "source" code (/src) is separated from the "distribution" code (/dist). 
* "source" code - written and edited 
* "distribution" code - minimized and optimized output of our build process that will eventually be loaded in the browser

`./node_modules/.bin/webpack src/index.js dist/bundle.js`

## Modules per Webpack documentation
The import and export statements have been standardized in ES2015. 
Although they are not supported in most browsers yet, webpack does support them out of the box.

Behind the scenes, webpack actually "transpiles" the code so that older browsers can also run it. 
If you inspect dist/bundle.js, you might be able to see how webpack does this, 
it's quite ingenious! Besides import and export, webpack supports various other module syntaxes as well, 
see Module API for more information.

**Note** webpack will not alter any code other than import and export statements. 
If you are using other ES2015 features, make sure to use a transpiler such as Babel or Bublé via webpack's loader system.

## Configuration File
`webpack.config.js`
Allows you to specify the entry point and how the output will be resolved in terms of filename and filepath via an exported module.
Now the project can be built using

`./node_modules/.bin/webpack --config webpack.config.js`

as opposed to 

`./node_modules/.bin/webpack src/index.js dist/bundle.js`

**Note** If a webpack.config.js is present, the webpack command picks it up by default. We use the --config option here only to show that you can pass a config of any name. This will come in useful for more complex configurations that need to be split into multiple files.

A configuration file allows far more flexibility than simple CLI usage. We can specify loader rules, plugins, resolve options and many other enhancements this way. 

