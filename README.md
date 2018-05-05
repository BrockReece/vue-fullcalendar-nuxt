# vue-fullcalendar-nuxt

> A nuxt project containing the vue-full-calendar plugin

## Build Setup

``` bash
# install dependencies
$ npm install # Or yarn install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, checkout the [Nuxt.js docs](https://github.com/nuxt/nuxt.js).

## Hints & Tips
### jQuery plugin
If your project uses jQuery, you will need to ensure that Nuxt will load it on the client side only. The following example will add a nuxt plugin that will load jQuery and jQuery-ui to your project.

```js
// plugins/jquery.js
import jquery from 'jquery'
import 'jquery-ui-bundle'

window.$ = window.jQuery = jquery
```

This plugin can then be included in your `nuxt.js` config file, with the `ssr` flag set to false. 

```js
// nuxt.js
plugins: [
    { src: '~/plugins/jquery', ssr: false }
],
```
