# Develop

## Create new component

Follow the [guidelines](https://github.com/geops/react-spatial/tree/master/src/components).

## Create new theme

Follow the [guidelines](https://github.com/geops/react-spatial/tree/master/src/themes).

## Documentation

We are using [react-styleguidist](https://react-styleguidist.js.org/).
Documentation and examples are available [here](https://react-spatial.geops.de/).

Build the doc:

```bash
yarn doc
```

Run the doc on [`locahost:6060`](http://locahost:6060/):

```bash
yarn start
```

## Tests

We are using [jest]([https://react-styleguidist.js.org/](https://jestjs.io/docs/en/getting-started.html)) and [enzyme]([https://github.com/airbnb/enzyme](https://airbnb.io/enzyme/)).

Run the tests in watch mode:

```bash
yarn test --watch
```

## Coverage

Run coverage:

```bash
yarn coverage
```

Then open the file `coverage/lcov-report/index.html` in your browser.

## Publish on [npmjs.com](https://www.npmjs.com/package/react-spatial)

Run publish:

```bash
publish:public
```
You need to enter the new version number in the command line.
Then the new version must be published on [npmjs.com](https://www.npmjs.com/package/react-spatial).

## Publish a development version on [npmjs.com](https://www.npmjs.com/package/react-spatial)

This version WILL NOT be displayed to other in [npmjs.com](https://www.npmjs.com/package/react-spatial).

Run publish:

```bash
publish:beta
```

You need to enter the new version number in the command line.
Append `-beta.0` to the current version or increase the beta number.
Then the new version must be published on [npmjs.com](https://www.npmjs.com/package/react-spatial) with the tag beta.

## How to use SVG

Before adding an SVG file in this folder please clean it using [svgo](https://www.npmjs.com/package/svgo) or [svgomg](https://jakearchibald.github.io/svgomg/).

After optimization, verify you can use it Openlayers using this [code sandbox](https://codesandbox.io/s/5w5o4mqwlk).

By default the svg loader will load the svg file as a react component if you want to load svg file as a base64 data URI use the extension .url.svg :

```javascript
// Import as a React component:
import NorthArrow from 'northArrow.svg';
// Use: <NorthArrow />

// Import as a base64 data URI (or with an url the file is too big):
import northArrow from 'northArrow.url.svg';
// northArrow = "data:image/svg+xml;base64,PHN2ZyB4...."
```

You could also use others loaders (ex: svg-inline-loader , svg-url-loader ...) .
To use these loaders just adapt the extension of the svg file and add the loader config in styleguide.config.js, then you will be able to use it like this :

```javascript
// Import as an inline svg:
import northArrow from 'northArrow.svginline.svg';
// northArrow = "<svg xmlns='http://www.w3.org/2000/svg' height='192' ...> ... </svg>"

// Import as an encoded inline svg data URI (or with an url the file is too big):
import northArrow from 'northArrow.svgurl.svg';
// northArrow = "data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' height='192' ...%3E ... %3C/svg%3E"
```

## How to use SVG with a dynamic size

In case you want your SVG fits perfectly his parent div you need to remove `width` and `height` attributes of `<svg>` and replace it by a `viewBox` property, like this:

Replace

    `<svg width="192" height="192">`

by

    `<svg viewBox="0 0 192 192">`