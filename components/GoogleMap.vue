<template>
  <div>
    <div ref="map"></div>
  </div>
</template>

<script>
import { Loader } from "@googlemaps/js-api-loader";
export default {
  props: ["center", "zoom", "geoJson", "mapStyle", "markers", "markerTool"],
  async mounted() {
    const loader = new Loader({
      apiKey: this.$config.googleApiKey,
      version: "weekly",
    });

    const loaddata = await loader.load()
    this.map = new google.maps.Map(this.$refs["map"], {
      center: this.center,
      zoom: this.zoom,
    });

    let marker = new google.maps.Marker({
      position: { lat: 58.41, lng: 23.32 },
      map: this.map,
      title: "Click to zoom",
    });
    this.markersPoint.push(marker);
    if (this.markerTool) {
      this.map.addListener("click", (e) => {
        this.placeMarker(e.latLng);
      });
    }
    if (!this.markerTool) {
      this.map.data.addGeoJson(this.geoJson);
    }

    this.map.data.setStyle(this.mapStyle);

  },
  data() {
    return {
      map: null,
      markersPoint: [],
      markeIsOn: false
    };
  },
  watch: {
    map(newval) {
    },
    center(newCenter) {
      this.map.panTo(newCenter);
    },
    zoom(newZoom) {
      this.map.setZoom(newZoom);
    },
    markerTool(newVal) {
      this.markeIsOn = true;
    },
    // geoJson(geoJson) {
    //   this.map.data.addGeoJson(this.geoJson);
    // },
    // mapStyle(mapStyle) {
    //   this.map.data.setStyle(this.mapStyle);
    // },

    markers(newMarkers) {
      this.map.data.addGeoJson();
      for(var i=0; i < this.markersPoint.length; i++){
        this.markersPoint[i].setMap(null);
      }
      for(var j=0; j < this.markers.length; j++){
        this.markers[j].setMap(null);
      }
      this.markers.forEach((marker) => {
        this.marker = new google.maps.Marker({
          position: marker.position,
          map: this.map,
        });
        this.markersPoint.push(this.marker)
      });
    },
  },
  methods: {
    placeMarker(location){
        for(var i=0; i < this.markersPoint.length; i++){
        }
        var marker = new google.maps.Marker({
          position: location,
          map: this.map
        });
        this.markersPoint.push(marker);
        // this.map.data.addGeoJson(this.geoJson);
    }
  },
};
</script>

<style scoped>
div {
  height: 800px;
}
</style>
