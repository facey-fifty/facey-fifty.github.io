---
---

<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/OrdnanceSurvey/os-api-branding@latest/os-api-branding.css" />
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<link rel="stylesheet" href="https://unpkg.com/maplibre-gl@5.24.0/dist/maplibre-gl.css" />


<script src="https://cdn.jsdelivr.net/gh/OrdnanceSurvey/os-api-branding@latest/os-api-branding.js"></script>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://unpkg.com/maplibre-gl@5.24.0/dist/maplibre-gl.js"></script>
<script src="https://unpkg.com/@maplibre/maplibre-gl-leaflet@0.1.3/leaflet-maplibre-gl.js"></script>
   <script src="https://unpkg.com/@mapbox/polyline@1.1.0/src/polyline.js"
   integrity="sha384-IDEhh/gOCXy+ASeq3Kr88jdsZ5XSXKUvq9WTPOGjKIZlEiZiJAZwd+GAnY3XDqLW"
   crossorigin=""></script>
<script src="https://unpkg.com/leaflet.icon.glyph@0.3.0/Leaflet.Icon.Glyph.js"></script>

<div id="map" style="height: 50vh">
</div>

<script>
    (function(L) {
       const routes = [
           {%- for route in site.the_routes -%}
            {
                "title": {{ route.title | jsonify }},
                "url": {{ route.url | relative_url | jsonify }},
                "polyline": {{ route.polyline  | jsonify }}
            }
            {%- unless forloop.last -%},{%- endunless -%}
            {%- endfor -%}
       ]

       const myMap = L.map('map').setView([53.6077, -1.8079], 12);     
       const gl = L.maplibreGL({
              style: 'https://api.os.uk/maps/vector/v1/vts?key=GfOkV72w6RR64DyYyLgI48arbXCxEdJK'
          }).addTo(myMap);
       
       let mapBounds = myMap.getBounds();
       for(var i = 0; i < routes.length; ++i) {
           const points = polyline.decode(routes[i].polyline);
           const start = L.marker(points[0], { icon: L.icon.glyph({prefix: '',glyph: '🚲'}) });
           const end = points[1];
           const layer = L.LayerGroup(
                start,
                L.polyline(points, { color: 'grey', weight: 3 })
           );
           mapBounds.extend(layer.getBounds());
           layer.addTo(myMap);
           layer.bindPopup("<a href='"+routes[i].url+"'>" + routes[i].title + "</a>")
           layer.on('popupopen', () => {
               layer.setStyle({color: 'red', weight: 5, opacity: 1 });
               myMap.fitBounds(layer.getBounds());
           });
           layer.on('popupclose', () => {
               layer.setStyle({color: 'grey', weight: 3 });
               myMap.fitBounds(mapBounds);
               
           });
       }
       myMap.fitBounds(mapBounds, { padding: [5, 5], animate: false });
    })(L);
</script>
