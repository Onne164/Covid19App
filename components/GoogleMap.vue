<template>
  <div ref="map"></div>
</template>

<script>
import { Loader } from "@googlemaps/js-api-loader"
export default {
  props: ['center', 'zoom', 'geoJson', 'mapStyle', 'markers'],
  mounted() {
    const loader = new Loader({
      apiKey: this.$config.googleApiKey,
      version: "weekly"
    });

    loader.load().then(() => {
      this.map = new google.maps.Map(this.$refs['map'], {
        center: this.center,
        zoom: this.zoom,
      });
      this.map.data.addGeoJson(this.geoJson);
      this.map.data.setStyle(this.mapStyle);
       // This event listener will call addMarker() when the map is clicked.

      });
    },
  data() {
    return {
      map: null,
    }
  },
  watch:{
    center(newCenter) {
        this.map.panTo(newCenter);
    },
    zoom(newZoom) {
        this.map.setZoom(newZoom);
    },
    geoJson(geoJson) {
      this.map.data.addGeoJson(this.geoJson);
    },
    mapStyle(mapStyle) {
      this.map.data.setStyle(mapStyle);
    },
    // markers(markers) {
    //   this.map.data.markers(markers);
    // }
  },
  methods: {

    }
}
</script>

<style scoped>
  div {
      height: 800px;
    }
</style>
