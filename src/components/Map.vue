<script setup lang="ts">
import { onMounted } from 'vue';
import 'mapbox-gl/dist/mapbox-gl.css';
import mapboxgl from 'mapbox-gl';
import restaurants from '@/json/restaurants';

function mapRender() {
  mapboxgl.accessToken = 'pk.eyJ1IjoiY2hhcmxlc2Nvc25lYXUiLCJhIjoiY2t6b2ZjYWpxMzgzMzJ1b2NweDVrMHpzaSJ9.T0lsMUbrV6kiaeBjA8wl3w';
  const map = new mapboxgl.Map({
  container: 'Mapbox',
  style: 'mapbox://styles/mapbox/streets-v11',
  center: [2.423085, 48.863374],
  zoom: 11
  });

  map.on('load', () => {
    map.addSource('borders', {
      type: 'geojson',
      data: ''
    });

    map.addLayer({
      id: 'bordersFill',
      type: 'fill',
      source: 'borders',
      paint: {
        'fill-color': '#FE5F55', 
        'fill-opacity': 0.7
      }
    });

    map.addLayer({
      id: 'bordersOutline',
      type: 'line',
      source: 'borders',
      paint: {
        'line-color': '#000',
        'line-width': 3
      }
    });

  })

  restaurants.forEach((restaurant: { lng: number; lat: number; }) => {
    const marker = new mapboxgl.Marker()
      .setLngLat([restaurant.lng, restaurant.lat])
      .addTo(map);
  })

}


onMounted(() => {
  mapRender();
})


</script>

<template>
  <div id="Mapbox"></div>
</template>

<style>
#Mapbox {
  width: 100vw;
  height: 100vh;
}
</style>