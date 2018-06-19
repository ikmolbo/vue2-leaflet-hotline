<template>
  <div style="display: none;">
    <slot v-if="ready"></slot>
  </div>
</template>

<script>
const L = require('leaflet')
const { findRealParent, propsBinder } = require('vue2-leaflet')
require('leaflet-hotline')(L)

const props = {
  latLngs: {
    type: Array,
    default: () => [],
    custom: false
  },
  min: {
    type: Number,
    custom: true
  },
  max: {
    type: Number,
    custom: true
  },
  weight: {
    type: Number,
    custom: true
  },
  outlineWidth: {
    type: Number,
    custom: true
  },
  outlineColor: {
    type: String,
    custom: true
  },
  palette: {
    type: Object,
    custom: true
  },
  visible: {
    type: Boolean,
    custom: true,
    default: true
  }
};

export default {
  name: 'LHotline',
  props,
  data () {
    return {
      ready: false
    }
  },
  mounted () {
    const options = {};

    if (this.min) {
      options.min = this.min;
    }
    if (this.max) {
      options.max = this.max;
    }
    if (this.weight) {
      options.weight = this.weight;
    }
    if (this.outlineWidth) {
      options.outlineWidth = this.outlineWidth;
    }
    if (this.outlineColor) {
      options.outlineColor = this.outlineColor;
    }
    if (this.palette) {
      options.palette = this.palette;
    }

    this.mapObject = L.hotline(this.latLngs, options);
    L.DomEvent.on(this.mapObject, this.$listeners);
    propsBinder(this, this.mapObject, props);
    this.ready = true;
    this.parentContainer = findRealParent(this.$parent);
    this.parentContainer.addLayer(this, !this.visible);
  },
  beforeDestroy () {
    this.parentContainer.removeLayer(this);
  },
  methods: {
    setMin (newVal, oldVal) {
      this.mapObject.setStyle({ min: newVal })
    },
    setMax (newVal, oldVal) {
      this.mapObject.setStyle({ max: newVal })
    },
    setWidth (newVal, oldVal) {
      this.mapObject.setStyle({ width: newVal })
    },
    setOutlineColor (newVal, oldVal) {
      this.mapObject.setStyle({ outlineColor: newVal })
    },
    setOutlineWidth (newVal, oldVal) {
      this.mapObject.setStyle({ outlineWidth: newVal })
    },
    setPalette (newVal, oldVal) {
      this.mapObject.setStyle({ palette: newVal })
    },
    setVisible (newVal, oldVal) {
      if (newVal === oldVal) return;
      if (newVal) {
        this.parentContainer.addLayer(this);
      } else {
        this.parentContainer.removeLayer(this);
      }
    },
    addLatLng (value) {
      this.mapObject.addLatLng(value);
    }
  }
};
</script>
