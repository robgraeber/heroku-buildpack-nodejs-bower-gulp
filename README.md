Heroku Buildpack for Node.js, Bower, and Gulp
========================================

Usage
-----

- Create a new app using `heroku create --buildpack=https://github.com/robgraeber/heroku-buildpack-nodejs-bower-gulp.git`. To be safe, you should fork this and use your fork's URL.
- Add a Gulp task that builds your app. By default, the buildpack will call `gulp build`. To specify your own task:
 - Run `heroku config:set GULP_TASK=dist` to set your gulp task to dist (or any other name)
- Add a `bower.json`/`.bowerrc` file that will be used with `bower install`. Packages not in `bower.json`are pruned each build.
- And additionally, the buildpack has a few extra entry points. It will look for entry points in the following order:  
 - `Procfile`: e.g. `web: node server.js`.
 - `npm start` script.
 - `server.js` file.
 - `app.js` file.

Note: It will only use bower/gulp if `bower.json` or `gulpfile.js` is found. Otherwise it's usable the same as other Node.js buildpacks, except with extra entry points.

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

Some Useful Aliases
-----------------------
I also like to create some useful aliases that help me deploy in 1-line. (Add the following to your .bash_profile):
```
alias heroku-create='heroku create --buildpack=https://github.com/robgraeber/heroku-buildpack-nodejs-bower-gulp.git'
autoDeploy() {
  heroku create $1 $2 $3 --buildpack=https://github.com/robgraeber/heroku-buildpack-nodejs-bower-gulp.git
  git push heroku master
  heroku open
}
alias heroku-create-auto=autoDeploy
alias heroku-portal='open https://dashboard.heroku.com/apps'
alias heroku-push='git push heroku master; heroku open'
```

Credits
-------

Forked from [heroku-buildpack-nodejs-gulp](https://github.com/timdp/heroku-buildpack-nodejs-gulp).

Which was forked from [heroku-buildpack-nodejs](https://github.com/heroku/heroku-buildpack-nodejs).

Heavily based on [heroku-buildpack-nodejs-grunt](https://github.com/mbuchetics/heroku-buildpack-nodejs-grunt).
