# svg.panzoom.js

> A plugin for [svg.js](ttps://github.com/svgdotjs/svg.js) that enables panzoom for viewport elements


## Getting started

```
npm install svg.panzoom.js
```

```js
var draw = SVG('id').size(1000,1000).panZoom()
```

panZoom() accepts optional min and max level of zooming by just passing {zoomMin: 0.5, zoomMax: 20}
to panZoom(), like this:

```js
var draw = SVG('id').size(1000,1000).panZoom({zoomMin: 0.5, zoomMax: 20})
```
Or at a later time by setting `zoomMin`/`zoomMax` directly on the element which is *panZoomed*.
```js
var draw = SVG('id').panZoom({zoomMax: 20})
draw.zoomMin = 1
```


The SVG element now support panning via mouse-down/mouse-move
and zooming via the mouse wheel. On touch devices, pinch-zoom
support allow you to pan and zoom in one gesture.

You can also animate zooming:

```js
draw.zoom(1) // uses center of viewport by default
    .animate()
    .zoom(2, {x:100, y:100}) // zoom into specified point
```

## Options

You can override the default options by passing an object in to the ``.panZoom({options})`` call.

| Option      | Default          | Description                                    |
| ----------- | ---------------- | ---------------------------------------------- |
| doPanning   | true             | Enable panning                                 |
| doPinchZoom | true             | Enable pinch to zoom                           |
| doWheelZoom | true             | Enable mouse wheel zoom                        |
| zoomFactor  | 0.03             | How quickly to zoom when using ``doWheelZoom`` |
| zoomMin     | 0                | The minimum zoom level                         |
| zoomMax     | Number.MAX_VALUE | The maximum zoom level                         |

### Example:

``` js
draw.panZoom({
  doWheelZoom: false,
  zoomMin: 0.5,
  zoomMax: 2
})
```

This will disable wheel zooming and set the maximum zoom to 2 or 200% and the minimum zoom to 0.5 or 50%.

## API

`svg.panzoom.js` adds the `.zoom()` method to all [viewbox][¹] elements.

- `zoom()` - returns current zoom level
- `zoom(Number)` - will zoom in or out depending if the Number is greater or less than the current zoom level
- `zoom(Number, {x,y})` - will zoom with the x/y coordinate as center point
- `zoom(Number, new SVG.Point(x,y))` - will zoom with the x/y coordinate as center point

| Method                             | Return Value |
| ---------------------------------- | ------------ |
| `zoom()`                           | Number       |
| `zoom(Number)`                     | element      |
| `zoom(Number, {x,y})`              | element      |
| `zoom(Number, new SVG.Point(x,y))` | element      |

[¹]: #viewbox-elements

### viewbox elements

- [SVG.Doc](http://svgjs.com/parents/#svg-doc)
- [SVG.Nested](http://svgjs.com/parents/#svg-nested)

## Events

Multiple events are fired doing different actions. This allow you to respond
to actions and in some cases stop an action via `preventDefault()`.

`zoom` is fired when a mouse wheel event or programmable `zoom()` triggers
a zoom. This usually doesn't happen on mobile devices, in which case
`pinchZoomStart` is fired when a zoom happens.

Events fired from SVG.js are [`CustomEvent`s](http://devdocs.io/dom/customevent),
so the arguments passed from svg.panzoom.js are in in the `.detail` property.

| Event Name     | Argument Value   | preventDefault support |
| -------------- | ---------------- | ---------------------- |
| zoom           | `{ box, focus }` | YES                    |
| panStart       | `{ event }`      | NO                     |
| panEnd         | `{ event }`      | NO                     |
| pinchZoomStart | `{ event }`      | YES                    |
| pinchZoomEnd   | `{ event }`      | NO                     |

Where [`box`](http://svgjs.com/geometry/#svg-box) is the new viewport,
[`focus`](http://svgjs.com/classes/#svg-point) is point of zoom
and event is the event that triggered the action.

An example of stopping a pan-zoom action:

```js
var draw = SVG('id').size(1000,1000).panZoom()
draw.on('pinchZoomStart', function(ev) {
    ev.preventDefault()
    ...
})
```
