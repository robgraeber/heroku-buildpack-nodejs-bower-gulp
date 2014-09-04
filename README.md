Heroku Buildpack for Node.js, Bower, and Gulp
========================================

Usage
-----

- Create a new app using `heroku create --buildpack=https://github.com/robgraeber/heroku-buildpack-nodejs-bower-gulp.git`. To be safe, you should fork this and use your fork's URL.
- Add a Gulp task called `build` that builds your app. For instance, the command called would be "gulp build"

- And either use a single line `Procfile` like: `web: node server.js`. Or it will look for either a npm start script, server.js, or app.js file to run. 


Things That Will Happen
-----------------------

When the buildpack runs it will do many things similarly to the standard [Heroku buildpack](https://github.com/heroku/heroku-buildpack-nodejs). In addition it will do the following things, not exactly in this order.

- If `gulpfile.js` is found
    - Run `npm install gulp` to install gulp locally during the build
    - Run `gulp build` to build the app
- If `bower.json` is found
    - Extract the `directory` key from `.bowerrc` if that is present
    - Run `npm update bower` to install bower locally
    - Run `bower install` to install `bower_components`. 
    - Cache `bower_components` or whichever alternate directory was specified in `.bowerrc`

The bower component caching is very similar to the node_modules caching done for npm. The cache is restored before each build and `bower prune` is run to remove anything no longer needed before doing the `bower install`. This is the same way the standard buildpack handles caching.

Credits
-------

Inspired by [Deploying a Yeoman/Angular app to Heroku](http://www.sitepoint.com/deploying-yeomanangular-app-heroku/).

Forked from [heroku-buildpack-nodejs-gulp](https://github.com/timdp/heroku-buildpack-nodejs-gulp).

Which was forked from [heroku-buildpack-nodejs](https://github.com/heroku/heroku-buildpack-nodejs).

Heavily based on [heroku-buildpack-nodejs-grunt](https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt).
