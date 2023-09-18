<template>
  <div>
    <div id="map" style="width: 100%; height: 400px;"></div>
  </div>
</template>

<script>
import 'leaflet/dist/leaflet.css'; // Importa los estilos de Leaflet
import L from 'leaflet';
import 'leaflet.heat/dist/leaflet-heat.js'; // Importa el plugin de mapa de calor de Leaflet

export default {
  mounted() {
    // Datos de ejemplo: coordenadas de ubicaciones en Matanzas, Cuba
    const locations = [
      [23.0445, -81.5775], // Ubicación 1
      [23.0423, -81.5801], // Ubicación 2
      [23.0478, -81.5786], // Ubicación 3
      [23.0452, -81.5777], // Ubicación 4
      [23.0431, -81.5773], // Ubicación 5
      [23.0465, -81.5779], // Ubicación 6
    ];

    // Inicializar el mapa de Leaflet
    const map = L.map('map').setView([23.0445, -81.5775], 15);

    // Agregar una capa de mapa (puedes usar otros proveedores de mapas)
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);

    // Crear una capa de mapa de calor con los datos
    L.heatLayer(locations).addTo(map);
  },
};
</script>

<style scoped>
/* Estilos personalizados para tu componente, si es necesario */
</style>
