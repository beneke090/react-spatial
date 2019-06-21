#

This demonstrates the use of CanvasSaveButton.

```jsx
import React from 'react';
import { TiImage } from 'react-icons/ti';
import OLMap from 'ol/Map';
import Layer from 'ol/layer/Layer';
import CanvasSaveButton from 'react-spatial/components/CanvasSaveButton';
import BasicMap from 'react-spatial/components/BasicMap';
import ConfigReader from 'react-spatial/ConfigReader';
import LayerService from 'react-spatial/LayerService';
import Copyright from '../Copyright/Copyright';
import NorthArrow from '../NorthArrow/NorthArrow';
import { toLonLat } from 'ol/proj';

const mbLayer = new Layer({
  render(frameState) {
    console.log('mbMap ', window.mbMap);
    if (!window.mbMap) {
      return;
    }
    const canvas = window.mbMap.getCanvas();
    const { viewState } = frameState;

    const visible = mbLayer.getVisible();
    canvas.style.display = visible ? 'block' : 'none';

    const opacity = mbLayer.getOpacity();
    canvas.style.opacity = opacity;

    // adjust view parameters in mapbox
    const { rotation } = viewState;
    if (rotation) {
      window.mbMap.rotateTo((-rotation * 180) / Math.PI, {
        animate: false,
      });
    }
    window.mbMap.jumpTo({
      center: toLonLat(viewState.center),
      zoom: viewState.zoom - 1,
      animate: false,
    });

    // cancel the scheduled update & trigger synchronous redraw
    // see https://github.com/mapbox/mapbox-gl-js/issues/7893#issue-408992184
    // NOTE: THIS MIGHT BREAK WHEN UPDATING MAPBOX
    if (window.mbMap._frame) {
      window.mbMap._frame.cancel();
      window.mbMap._frame = null;
    }
    window.mbMap._render();

    return canvas;
  },
});
const config =  [
    {
      name: 'Basemap',
      visible: true,
      isBaseLayer: true,
      data: {
        type: 'custom',
        layer: mbLayer,
      },
    },
  ];
class CanvasSaveButtonExample extends React.Component {
  constructor(props) {
    super(props);
    this.map = new OLMap({ controls: [] });
    this.center = [874865.46, 5931211.65];

    const layers = ConfigReader.readConfig(
      this.map,
      config,
    );

    this.layerService = new LayerService(layers);
  }

  componentDidMount() {
    window.mbMap = new window.mapboxgl.Map({
      style: `https://vtiles.prod.geops.ch/styles/inspirationskarte.json`,
      attributionControl: false,
      boxZoom: false,
      center: toLonLat(this.center),
      container: 'map',
      doubleClickZoom: false,
      dragPan: false,
      dragRotate: false,
      interactive: false,
      keyboard: false,
      pitchWithRotate: false,
      scrollZoom: false,
      touchZoomRotate: false,
      preserveDrawingBuffer: true,
      trackResize:false
    });
    map.getView().setCenter(this.center);
  }

  render() {

    return (
      <div className="tm-canvas-save-button-example">
        <BasicMap
          map={this.map}
          center={this.center}
          zoom={9}
        />
        <CanvasSaveButton
          title="Karte als Bild speichern."
          className="tm-round-grey-hover-primary tm-button"
          map={this.map}
          extraData={{
            copyright: {
              text: () => {
                return this.layerService.getCopyrights();
              },
            },
            northArrow: {
              rotation: () => {
                return NorthArrow.radToDeg(this.map.getView().getRotation());
              },
              circled: true,
            },
          }}
        >
          <TiImage focusable={false} />
        </CanvasSaveButton>
        <CanvasSaveButton
          title="Karte als Bild speichern."
          className="tm-round-grey-hover-primary tm-button"
          map={this.map}
          extraData={{
            dpi:300,
            copyright: {
              text: () => {
                return this.layerService.getCopyrights();
              },
            },
            northArrow: {
              rotation: () => {
                return NorthArrow.radToDeg(this.map.getView().getRotation());
              },
              circled: true,
            },
          }}
        >
          <TiImage focusable={false} />
        </CanvasSaveButton>
      </div>
    );
  }
}

<CanvasSaveButtonExample />;
```
