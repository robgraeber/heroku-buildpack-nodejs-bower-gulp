Heroku Buildpack for Node.js, Bower, and Gulp
========================================

Usage
-----

- Create a new app using `heroku create --buildpack=https://github.com/robgraeber/heroku-buildpack-nodejs-bower-gulp.git`. To be safe, you should fork this and use your fork's URL.
- Add a Gulp task called `build` that builds your app. For instance, the command called will be "gulp build"
- Add a `bower.json`/`.bowerrc` file that will be used with `bower install`. Packages not in `bower.json`are pruned each build.
- And finally, the buildpack has a few default entry points. It will look for entry points in the following order:  
 - `Procfile`: e.g. `web: node server.js`.
 - `npm start` script.
 - `server.js` file.
 - `app.js` file.


Things That Will Happen
-----------------------

When the buildpack runs it will do many things similarly to the standard [Heroku buildpack](https://github.com/heroku/heroku-buildpack-nodejs). In addition it will do the following things, in roughly this order.

- If `bower.json` is found
    - Extract the `directory` key from `.bowerrc` if that is present
    - Run `npm install bower` to install bower locally
    - Run `bower install` to install `bower_components`. 
    - Cache `bower_components` or whichever alternate directory was specified in `.bowerrc`
- If `gulpfile.js` is found
    - Run `npm install gulp` to install gulp locally during the build
    - Run `gulp build` to build the app
- If `Procfile` is not found
    - Looks for a `npm start` script
    - Then for a `server.js` file
    - And then for an `app.js` file

The bower component caching is very similar to the node_modules caching done for npm. The cache is restored before each build and `bower prune` is run to remove anything no longer needed before doing the `bower install`. This is the same way the standard buildpack handles caching.

Credits
-------

Inspired by [Deploying a Yeoman/Angular app to Heroku](http://www.sitepoint.com/deploying-yeomanangular-app-heroku/).

Forked from [heroku-buildpack-nodejs-gulp](https://github.com/timdp/heroku-buildpack-nodejs-gulp).

Which was forked from [heroku-buildpack-nodejs](https://github.com/heroku/heroku-buildpack-nodejs).

Heavily based on [heroku-buildpack-nodejs-grunt](https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt).
