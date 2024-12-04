<!DOCTYPE html>
<html>
<head>
  <title>Baltimore Dorling Cartogram</title>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Load Leaflet CSS and JavaScript -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
  <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>

  <!-- Load Leaflet-Awesome-Markers CSS and JavaScript (optional for styling) -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet.awesome-markers/2.0.4/leaflet.awesome-markers.css" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet.awesome-markers/2.0.4/leaflet.awesome-markers.js"></script>

  <style>
    #map {
      height: 100vh;
    }
  </style>
</head>
<body>
  <div id="map"></div>

  <script>
    // Initialize the map and set its view to the geographic center of Baltimore
    var map = L.map('map').setView([39.2904, -76.6122], 11);

    // Add a base map layer (OpenStreetMap)
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 18,
      attribution: '© OpenStreetMap contributors'
    }).addTo(map);

    // Load the GeoJSON data
    fetch('baltimore_dorling.geojson')
      .then(function(response) {
        return response.json();
      })
      .then(function(geojsonData) {
        // Define a color mapping for cluster labels
        var clusterColors = {
          "Affluent Suburban Communities": "#1f78b4",
          "Diverse Middle-Class Neighborhoods": "#33a02c",
          "Low-Income Urban Areas": "#e31a1c",
          "Urban Professionals": "#ff7f00",
          "Emerging Neighborhoods": "#6a3d9a",
          "Other": "#b15928"
        };

        // Add the GeoJSON layer to the map
        L.geoJSON(geojsonData, {
          style: function(feature) {
            return {
              color: 'white',
              weight: 1,
              fillOpacity: 0.7,
              fillColor: clusterColors[feature.properties.cluster_label] || '#b15928'
            };
          },
          onEachFeature: function(feature, layer) {
            // Bind a popup displaying cluster information
            layer.bindPopup('<strong>Cluster:</strong> ' + feature.properties.cluster_label);
          }
        }).addTo(map);
      });
  </script>
</body>
</html>
