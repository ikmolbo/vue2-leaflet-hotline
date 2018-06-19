# vue2-leaflet-hotline

This is a [hotline plugin](https://github.com/iosphere/Leaflet.hotline) extension for [vue2-leaflet package](https://github.com/KoRiGaN/Vue2Leaflet)

## Install

    npm install --save vue2-leaflet-hotline

## Usage

### In &lt;template&gt; add

something like this

    <v-map :zoom=10 :center="initialLocation">
      <v-tilelayer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></v-tilelayer>
      <v-hotline :latlngs="latLngArray" :min="minValue" :max="maxValue"></v-hotline>
    </v-map>

### In &lt;script&gt; add

#### option 1

In the same template file, at `<script>` part, this will make the component available only to the template in this file

    import Vue2LeafletHotline from 'vue2-leaflet-hotline'
    ...
    export default {
      ...
      components: {
        'v-hotline': Vue2LeafletHotline
        ...
      },
      ...
    }

#### option 2

At main Vue configuration, this will make the component available to all templates in your app

    import Vue from 'vue'
    import Vue2LeafletHotline from 'vue2-leaflet-hotline'
    ...
    Vue.component('v-leaflet-hotline', Vue2LeafletHotline)

### `latlngs` prop

The `latlngs` prop needs to be an array of `LatLng` points (a polyline) with an additional third element (z value) in each point; this determines which color from the `palette` to use.

### Other props

You can use the following props to style the hotline:

- **weight** - Same as usual. `5` per default.
- **outline-width** - The width of the outline along the stroke in pixels. Can be `0`. `1` per default.
- **outline-color** - The color of the outline. `'black'` per default.
- **palette** - The config for the palette gradient in the form of `{ <stop>: '<color>' }`. `{ 0.0: 'green', 0.5: 'yellow', 1.0: 'red' }` per default. Stop values should be between `0` and `1`.
- **min** - The smallest z value expected in the `latlngs` array. This maps to the `0` stop value. Any z values smaller than this will be considered as `min` when choosing the color to use.
- **max** - The largest z value expected in the `latlngs` array. This maps to the `1` stop value. Any z values greater than this will be considered as `max` when choosing the color to use.

## Author

[Ilya Oblomov](https://www.getairbase.com)

## Acknowledgements

Thanks to iosphere GmbH, the authors of the Leaflet.Hotline plugin. This package has been used as the basis to build this plugin.

## License

MIT
