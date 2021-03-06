<!-- INJECT:"LineLayerDemo" -->

<p class="badges">
  <img src="https://img.shields.io/badge/64--bit-support-blue.svg?style=flat-square" alt="64-bit" />
</p>

# LineLayer

The Line Layer renders flat lines joining pairs of source and target points,
specified as latitude/longitude coordinates.

```js
import DeckGL, {LineLayer} from 'deck.gl';

const App = ({data, viewport}) => {

  /**
   * Data format:
   * [
   *   {
   *     inbound: 72633,
   *     outbound: 74735,
   *     from: {
   *       name: '19th St. Oakland (19TH)',
   *       coordinates: [-122.269029, 37.80787]
   *     },
   *     to: {
   *       name: '12th St. Oakland City Center (12TH)',
   *       coordinates: [-122.271604, 37.803664]
   *   },
   *   ...
   * ]
   */
  const layer = new LineLayer({
    id: 'line-layer',
    data,
    pickable: true,
    getWidth: 50,
    getSourcePosition: d => d.from.coordinates,
    getTargetPosition: d => d.to.coordinates,
    getColor: d => [Math.sqrt(d.inbound + d.outbound), 140, 0],
    onHover: ({object, x, y}) => {
      const tooltip = `${object.from.name} to ${object.to.name}`;
      /* Update tooltip
         http://deck.gl/#/documentation/developer-guide/adding-interactivity?section=example-display-a-tooltip-for-hovered-object
      */
    }
  });

  return (<DeckGL {...viewport} layers={[layer]} />);
};
```

## Properties

Inherits from all [Base Layer](/docs/api-reference/layer.md) properties.

### Render Options

The width used to draw each line. Unit is pixels.

##### `fp64` (Boolean, optional)

* Default: `false`

Whether the layer should be rendered in high-precision 64-bit mode. Note that since deck.gl v6.1, the default 32-bit projection uses a hybrid mode that matches 64-bit precision with significantly better performance.

##### `widthUnits` (String, optional)

* Default: `'pixels'`

The units of the line width, one of `'meters'`, `'pixels'`. When zooming in and out, meter sizes scale with the base map, and pixel sizes remain the same on screen.

##### `widthScale` (Number, optional)

* Default: `1`

The scaling multiplier for the width of each line. This prop is a very efficient way to change the width of all objects, comparing to recalculating the width for each object with `getWidth`.

##### `widthMinPixels` (Number, optional)

* Default: `1`

The minimum line width in pixels. This prop can be used to prevent the line from getting to thin when zoomed out.

##### `widthMaxPixels` (Number, optional)

* Default: `Number.MAX_SAFE_INTEGER`

The maximum line width in pixels. This prop can be used to prevent the line from getting to thick when zoomed in.


### Data Accessors

##### `getSourcePosition` (Function, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `object => object.sourcePosition`

Method called to retrieve the source position of each object.

##### `getTargetPosition` (Function, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `object => object.targetPosition`

Method called to retrieve the target position of each object.

##### `getColor` (Function|Array, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `[0, 0, 0, 255]`

The rgba color of each object, in `r, g, b, [a]`. Each component is in the 0-255 range.

* If an array is provided, it is used as the color for all objects.
* If a function is provided, it is called on each object to retrieve its color.

##### `getWidth` (Function|Number, optional) ![transition-enabled](https://img.shields.io/badge/transition-enabled-green.svg?style=flat-square")

* Default: `1`

The line width of each object, in units specified by `widthUnits` (default pixels).

* If a number is provided, it is used as the line width for all objects.
* If a function is provided, it is called on each object to retrieve its line width.

## Source

[modules/layers/src/line-layer](https://github.com/uber/deck.gl/tree/master/modules/layers/src/line-layer)

