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

    markers(newMarkers) {
      newMarkers.forEach(marker => {
        this.marker =  new google.maps.Marker({
          position: {lat: 59.464697449279925, lng:24.8273948065468},
          map: this.map,
        });
      });
    }


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
