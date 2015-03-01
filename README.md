# ES6 Isomorphic Flux/ReactJS Boilerplate

> A wonderfull boilerplate for **Flux/ReactJS** applications, running on **ExpressJS**.

## Libraries Included

* [react](https://facebook.github.io/react/)
* [react-router](https://github.com/rackt/react-router)
* [react-hot-loader](https://github.com/gaearon/react-hot-loader)
* [alt](https://github.com/goatslacker/alt)
* [iso](https://github.com/goatslacker/iso)
* [expressjs](http://expressjs.com/)
* [webpack](http://webpack.github.io/)
* [gulpjs](http://gulpjs.com/)
* [babeljs](https://babeljs.io/)

## Why use this boilerplate?

I wanted to create a simple isomorphic reactjs application, but I found [ambidex](https://github.com/appsforartists/ambidex) or [fluxible](https://github.com/yahoo/fluxible) to hard to dive in, too many concepts and it's kinda killing the way of React thinking. Thinking as components, little bricks to assemble.

I needed also an complete stack ready for full web-app development, with nice tools for the front-end. (Gulp and his magic tasks).

With this boilerplate, you can choose easily what components you need and you won't be lost.

## Concepts

**ExpressJS** will be our server for the server side rendering, we use **alt** for our Flux architecture and **react-router** for routing in our app.

With **iso** as helper we can populate **alt** flux stores before the first rendering and have a complete async isomorphic React application.

Run this boilerplate, you will see the server is fetching some fake users and will populate the `UserStore` with this data. **ExpressJS** will render the first markup, serve the JavaScript and then it will entirely run on the client.

For now, you will have to declare middlewares in your **ExpressJS** application for fetching the data. But at term it should be easier and share the same logic for fetching data on client and server. (Maybe with [react-resolver](https://github.com/ericclemmons/react-resolver), I'm still exploring ways).

## Alt-resolver

Alt-resolver is used to resolve data before React Rendering, it shares the same services with client and server. It's specific to the boilerplate.

Presume you have a route like this:

```
<Route name="users" handler={require('./components/users')} />
```

And you need to fetch users and populate an UserStore before rendering the component, declare an `user` function (that match the route name) in `shared/service.js`.

This function take a callback called `done` with first argument is the `error` (should be null) and second is a formatted object for `Iso / Alt` like this:

```
users(done) {
  return request
    .get('http://api.randomuser.me/?results=10')
    .end((response) => {
      return done(null, {
        UserStore: {users: response.body.results}
      });
    });
}
```

* On server side, `users` function will be resolved before the first rendering.
* On client-side when switching to the route, it will be resolved before rendering too!

## Installation / How-to

I recommend to use [io.js](https://iojs.org/) to take advantages of `ES6` without `--harmony` flag on `NodeJS`.

**Note:** We're using [io.js](https://iojs.org/) **1.2.0**. `node-sass` (`gulp-sass` dependency) **does not yet support io.js > 1.2.0**. See [#699](https://github.com/sass/node-sass/issues/699)

It's super easy to do with [nvm](https://github.com/creationix/nvm):

* `$ nvm install iojs-v1.2.0`
* `$ nvm use iojs-v1.2.0`
* `$ nvm alias default iojs-v1.2.0` (to make `node` default to `iojs-v1.2.0`)

After that, you will just need to clone the repo and install dependancies:

* `$ git clone https://github.com/iam4x/iso-react.git`
* `$ cd iso-react && npm install`
* `$ npm install -g gulp`

### Run the project in development:

* `$ gulp dev`

Open your browser to `http://localhost:8080` and you will see the magic happens! Try to disable JavaScript in your browser, you will still be able to navigate between pages of the application. Enjoy the power of isomorphic applications!

### Build project:

Just run `$ gulp build`, it will produce these tasks:

* Concat & minify styles to `/dist/css/styles.css`
* Concat & minify scripts to `/dist/js/app.js`
* Optimize & copy images to `/dist/img/`
