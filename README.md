# Universal React

This boilerplate aims at solving the MVP (Minimal Viable Product) of a universal app while trying to keep the base unopinionated elsewhere and simple to read and extend.

[![Universal React on NPM](https://img.shields.io/npm/v/universal-react.svg)](https://www.npmjs.com/package/universal-react)
[![Dependency Status](https://david-dm.org/DominicTobias/universal-react.svg)](https://david-dm.org/DominicTobias/universal-react)

## Features

- Universal routing [react-router](https://github.com/rackt/react-router)
- [Redux](https://rackt.github.io/redux/)
- Hot reloading
- Title, meta, css, and scripts overridable by each component [react-helmet](https://github.com/nfl/react-helmet)
- Universal data fetching/rehydration on the client [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch)
- No other templating engines - React from root down
- 404 and redirect handling
- Shared app config
- [Webpack](https://webpack.github.io) and [Babel](https://babeljs.io)

Since there are so many opinions on how to use css (vanilla, sass, less, react css modules etc) I've left it up to you.

## Install & run

```
npm i && npm start
```

Go to `http://localhost:3000/`.

## Build

```
npm run build
```

This will create a `dist/` folder with a `app.min.js` which will be used on any environment which isn't undefined (i.e. not local).

```
npm run start-prod
```

This will build and then run your app with environment set to production, so that `app.min.js` and `config/production.js` is used.

## Adding routes

Add your routes in `Routes.js`.

```js
<Route path='users' component={Users} />
```

## Title and Meta

The parent `App.js` defines the base title and meta in a `Helmet` component. Any sub-component can override/add properties (even adding scripts and css). See the [react-helmet docs](https://github.com/nfl/react-helmet) for more info.

## Config

You can store app settings under `app/config/`. A file matching `process.env.NODE_ENV` will be loaded, for example `app/config/production.js`. If `process.env.NODE_ENV` is undefined it will fallback to `app/config/default.js`. You can access the correct config with:

```js
import config from './config';
```

## Data fetching and client hydration

Read the [Redux](https://rackt.github.io/redux/) guide if you are new to redux. Write Redux actions and stores as normal, and if the action creator is asynchronous then it should return a [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) (or a [Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)) in the inner function.

On a container you need to declare the actions in an array that must be executed in order for it to be ready:

```js
static readyOnActions(dispatch, params) {
	return return Promise.all([
		dispatch(UserActions.fetchUserIfNeeded(params.id))
	]);
}
```

You should also invoke the actions in `componentDidMount` (or the constructor or `componentWillMount`). This ensures that if the component is reached on the client, then the same actions will be invoked:

```js
componentWillMount() {
	User.readyOnActions(this.props.dispatch, this.props.params);
}
```
