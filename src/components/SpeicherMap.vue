<template>
  <div class="map-container">
    <l-map :zoom="zoom" :center="center" :options="mapOptions" @zoomend="handleZoom">
      <l-image-overlay :url="mapImageUrl" :bounds="mapBounds"></l-image-overlay>
      <l-marker v-for="marker in markers" :key="marker.id" :lat-lng="marker.coordinates" @click="handleMarkerClick(marker)">
        <l-popup>{{ marker.text }}</l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LImageOverlay, LMarker, LPopup } from "@vue-leaflet/vue-leaflet";
import "leaflet/dist/leaflet.css";
import L from "leaflet";

const w = 2707/5;
const h = 6555/5;

export default {
  components: {
    LMap,
    LImageOverlay,
    LMarker,
    LPopup,
  },
  data() {
    return {
      zoom: 0.00001 , // Adjust the initial zoom level as needed
      center: [w/2, h/2], // Adjust the initial center coordinates as needed
      mapBounds: [[0, 0], [w, h]], // Adjust the map bounds based on the image dimensions
      mapImageUrl: 'map.png', // Replace with the path to your map image
      markers: [
        { id: 1, coordinates: [100, 100], text: 'Marker 1' }, // Example marker, replace with your markers
        { id: 2, coordinates: [200, 200], text: 'Marker 2' }, // Example marker, replace with your markers
      ],
      mapOptions: {
        crs: L.CRS.Simple, // Use Simple CRS for non-geographical maps
        attributionControl: false,
        maxBoundsViscosity: 1.0,
        maxBounds: [[0, 0], [w, h]],
      },
    };
  },
  methods: {
    handleMarkerClick(marker) {
      // Center the map on the clicked marker
      this.center = marker.coordinates;
      //this.zoom = 5; // Adjust the zoom level as desired
    },
    handleZoom(event) {
      // Update the zoom level when the map is zoomed
      this.zoom = event.target.getZoom();
    },
  },
};
</script>

<style scoped>
.map-container {
  width: 100%;
  height: 100vh;
}
</style>