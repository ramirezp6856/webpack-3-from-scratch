# webpack-3-from-scratch

# Getting Started
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

A configuration file allows far more flexibility than simple CLI usage. We can specify loader rules, plugins, resolve options and many other enhancements this way. For more info read: https://webpack.js.org/configuration/

## NPM Scripts
`Package.json` was updated by adding an npm script to handle building webpack.

Now the `npm run build` command can be used in place of the longer commands used earlier.

# Asset Management
Webpack asset management can allow projects to incorporate assets, like images.

Prior to webpack, front-end developers would use tools like grunt and gulp to process these assets and move them from their `/src` folder into their `/dist` or `/build` directory. The same idea was used for JavaScript modules, but tools like webpack will dynamically bundle all dependencies (creating what's known as a dependency graph). This is great because every module now explicitly states its dependencies and we'll avoid bundling modules that aren't in use.

One of the coolest webpack features is that you can also include any other type of file, besides JavaScript, for which there is a loader. This means that the same benefits listed above for JavaScript (e.g. explicit dependencies) can be applied to everything used in building a website or web app. Let's start with CSS, as you may already be familiar with that setup.

## Loading CSS
In order to import a CSS file from within a JavaScript module, you need to install and add the style-loader and css-loader to your module configuration:
`npm install --save-dev style-loader css-loader`
These are then added to the package.json file. So, `npm install` will include them.

`webpack.config.js` 

**Note** webpack uses a regular expression to determine which files it should look for and serve to a specific loader. In this case any file that ends with .css will be served to the style-loader and the css-loader.

## Loading Images
Using the `file-loader` we can easily incorporate images.
`npm install --save-dev file-loader`

The following stackoverflow was very helpful!

https://stackoverflow.com/questions/37671342/how-to-load-image-files-with-webpack-file-loader

## Loading Data
nother useful asset that can be loaded is data, like JSON files, CSVs, TSVs, and XML. Support for JSON is actually built-in, similar to NodeJS, meaning `import Data from './data.json'` will work by default. To import CSVs, TSVs, and XML you could use the csv-loader and xml-loader. Let's handle loading all three:

`npm install --save-dev csv-loader xml-loader`

When you open index.html and look at your console in your developer tools, you should be able to see your imported data being logged to the console!

This can be especially helpful when implementing some sort of data visualization using a tool like d3. Instead of making an ajax request and parsing the data at runtime you can load it into your module during the build process so that the parsed data is ready to go as soon as the module hits the browser.

# Branch Asset Management in Webpack 
The branch Asset Management in Webpack was created right after going through the Webpack documentation for Asset Management. The **Wrapping Up** section recommended to delete some of the configurations in the index.js and the webpack config file. Also, had to delete project files.

# Output Management
So far we've manually included all our assets in our index.html file, but as your application grows and once you start using hashes in filenames and outputting multiple bundles, it will be difficult to keep managing your index.html file manually. However, there's no need to fear as a few plugins exist that will make this process much easier to manage.

We can see that webpack generates our print.bundle.js and app.bundle.js files, which we also specified in our index.html file. if you open index.html in your browser, you can see what happens when you click the button.


