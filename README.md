## Mustache loader for [webpack](https://webpack.github.io/)

Compiles [Mustache](https://mustache.github.io/) templates with [Hogan](https://twitter.github.io/hogan.js/) and optionally [html-minifier](https://github.com/kangax/html-minifier).

### Install

```sh
$ npm i -S webpack-mustache-loader
```

### Usage

webpack@1.x
```javascript
module: {
    loaders: [ {
        test: /\.html$/,
        loader: 'webpack-mustache-loader'
        // loader: 'mustache?minify'
        // loader: 'mustache?{ minify: { removeComments: false } }'
        // loader: 'mustache?noShortcut'
    } ]
}
```
webpack@2.x
```javascript
module: {
    rules: [ {
        test: /\.html$/,
        loader: 'webpack-mustache-loader'
        // loader: 'mustache-loader?minify'
        // loader: 'mustache-loader?{ minify: { removeComments: false } }'
        // loader: 'mustache-loader?noShortcut'
    } ]
}
```

```javascript
var template = require('./template.html');
var html = template({ foo: 'bar' });
```

If `noShortcut` is passed, then Hogan compiled template is returned instead, so
you can pass it as partial.

```javascript
var template = require('./template.html');
var template2 = require('./template2.html');
var html = template.render({ foo: 'bar' }, {partial: template2});
```

If `clientSide` is passed in, then Hogan will not pre-compile the template.

If `tiny` is passed in, the source of the template will not be emitted, creating a smaller output.

if `render` is passed in, the data is sent is used to immediately render the template.  Render may be an object or a function which returns an object (in order to allow the data to change over time, e.g. to support hot reloading).

For example, the following will render `index.mustache` with the provided data (`title`), which can immediately be used by HtmlWebpackPlugin.

```javascript
module: {
    rules: [ {
        test: /index\.mustache$/,
        loader: 'webpack-mustache-loader',
        options: {
            tiny: true,
            render: {
                title: 'hello world',
            },
        },
    } ]
}
plugins: [
    new HtmlWebpackPlugin({
        template: 'index.mustache',
        inject: 'body',
    }),
]
```

If another loader is chained after Mustache-Loader then the `minify`, `clientSide`, and `tiny` options will be ignored.

Any additional Hogan parameters passed into this loader will be passed through to Hogan.

[Documentation: Using loaders](https://webpack.github.io/docs/using-loaders.html).

### License
[WTFPL](http://www.wtfpl.net/wp-content/uploads/2012/12/wtfpl-strip.jpg)
