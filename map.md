---
---

<div id="map" style="height: 400px"></div>


<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/OrdnanceSurvey/os-api-branding@latest/os-api-branding.css" />
<link rel="stylesheet" href="https://unpkg.com/maplibre-gl@^6.9.1/dist/maplibre-gl.css" />



<script src="https://cdn.jsdelivr.net/gh/OrdnanceSurvey/os-api-branding@latest/os-api-branding.js"></script>
<div id="the-map" style="height: 50vh">
</div>
<script type="module">
    import * as maplibregl from 'https://unpkg.com/maplibre-gl@6.9.1/dist/maplibre-gl.mjs';
    import polyline from 'https://esm.unpkg.com/@mapbox/polyline@1.2.1/src/polyline.js';
    const apiKey = 'GfOkV72w6RR64DyYyLgI48arbXCxEdJK';

    const map = new maplibregl.Map({
        container: 'map',
        minZoom: 6,
        maxZoom: 18,
        style: 'https://api.os.uk/maps/vector/v1/vts/resources/styles?srs=3857&key=' + apiKey,
        maxBounds: [
            [ -10.76418, 49.528423 ],
            [ 1.9134116, 61.331151 ]
        ],
        center: [ -2.968, 54.425 ],
        zoom: 13,
        attributionControl: false
    });

    map.dragRotate.disable(); // Disable map rotation using right click + drag.
    map.touchZoomRotate.disableRotation(); // Disable map rotation using touch rotation gesture.

    // Add navigation control (excluding compass button) to the map.
    map.addControl(new maplibregl.NavigationControl({
        showCompass: false
    }));
</script>
<!--
   const routes = [
        {%- for route in site.the_routes -%}
         {
             "title": {{ route.title | jsonify }},
             "url": {{ route.url | relative_url | jsonify }},
             "polyline": {{ route.polyline  | jsonify }}
         }
         {%- unless forloop.last -%},{%- endunless -%}
         {%- endfor -%}
    ];

    const map = new maplibregl.Map({
        container: 'the-map', // container id
        style: 'https://api.os.uk/maps/vector/v1/vts/resources/styles?srs=3857&key=GfOkV72w6RR64DyYyLgI48arbXCxEdJK', // style URL
        center: [53.6077, -1.8079], // starting position [lng, lat]
        zoom: 12 // starting zoom
    });

    map.on('load', () => {
       for(var i = 0; i < routes.length; ++i) {
          let route = routes[i];
          map.addSource(`route-${i}`, {
                 'type': 'geojson',
                 'data': {
                     'type': 'Feature',
                     'properties': {},
                     'geometry': {
                         'type': 'LineString',
                         'coordinates': polyline.decode(route.polyline)
                     }
                 }
             });
          map.addLayer({
              'id': `route-${i}`,
              'type': 'line',
              'source': `route-${i}`,
              'paint': {
                  'line-color': '#888',
                  'line-width': 2
              }
          });
       }
    });
</script>

-->
