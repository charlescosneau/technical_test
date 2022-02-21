<script setup lang="ts">
import { onMounted } from 'vue'
import 'mapbox-gl/dist/mapbox-gl.css'
import mapboxgl from 'mapbox-gl'
import restaurants from '@/../public/json/restaurants'



function bordersRender(map: mapboxgl.Map) {
  map.on('load', () => {
    map.addSource('borders', {
      type: 'geojson',
      data: 'http://localhost:3000/json/borders.json'
    })
    map.addLayer({
      id: 'bordersOutline',
      type: 'line',
      source: 'borders',
      paint: {
        'line-color': '#000',
        'line-width': 3
      }
    })
  })
  map.flyTo({
    center: [5.413938, 49.047639],
    speed: 0.5,
    zoom: 8.2
  })
}

function restaurantsMarker(map: mapboxgl.Map) {
  restaurants.forEach((restaurant: { lng: number; lat: number; }) => {
    const marker = new mapboxgl.Marker()
      .setLngLat([restaurant.lng, restaurant.lat])
      .addTo(map);
  })
}

// function restaurantsMarker(map: mapboxgl.Map) {
//   map.on('load', () => {
//     restaurants.forEach((restaurant: { lng: number; lat: number; }, index: number) => {
//       map.addSource("restaurants", {
//         "type": "geojson",
//         "data": {
//           "type": "Feature",
//           "geometry": {
//             "type": "Point",
//             "coordinates": [restaurant.lng, restaurant.lat]
//           },
//           "properties": {
//             "title": "Restaurants",
//             "marker-symbol": "restaurant"
//           }
//         },
//         cluster: true,
//       })
//     })
//   })
// }


function fibreRender(map: mapboxgl.Map) {
  map.on('load', () => {
    map.addSource('fibre', {
      type: 'geojson',
      data: 'http://localhost:3000/json/fibre.json'
    });
    map.addLayer({
      id: 'fibreFill',
      type: 'fill',
      source: 'fibre',
      filter: ['has', 'couv'],
      paint: {
        'fill-color': [
          'step',
          ['get', 'couv'],
          '#FFF496',
          20,
          '#CCB800',
          40,
          '#FA8E00',
          60,
          '#BA6900',
          80,
          '#BA0000',
          ] 
      }
    })
  })
}

onMounted(() => {
  mapboxgl.accessToken = '<mapboxgl-accesstoken>';
  const map = new mapboxgl.Map({
    container: 'Mapbox',
    style: 'mapbox://styles/mapbox/streets-v11',
    center: [2.423085, 48.863374],
    zoom: 8
  });
  bordersRender(map)
  fibreRender(map)
  restaurantsMarker(map)
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
.filter {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  color: black;
}
</style>