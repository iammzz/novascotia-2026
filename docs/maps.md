# 🗺️ Interactive Maps & GPS Waypoint Database

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>

<div style="margin-bottom: 15px; display: flex; gap: 8px; flex-wrap: wrap;">
  <button onclick="filterMarkers('All')" style="padding: 6px 14px; border-radius: 20px; border: 1px solid #ccc; background: #fff; cursor: pointer; font-weight: bold;">Show All</button>
  <button onclick="filterMarkers('Base')" style="padding: 6px 14px; border-radius: 20px; border: 1px solid #E53935; background: #FFEBEE; color: #C62828; cursor: pointer; font-weight: bold;">🔴 Base Camps</button>
  <button onclick="filterMarkers('Hike')" style="padding: 6px 14px; border-radius: 20px; border: 1px solid #43A047; background: #E8F5E9; color: #2E7D32; cursor: pointer; font-weight: bold;">🟢 Hikes</button>
  <button onclick="filterMarkers('Activity')" style="padding: 6px 14px; border-radius: 20px; border: 1px solid #FB8C00; background: #FFF3E0; color: #EF6C00; cursor: pointer; font-weight: bold;">🟠 Activities & Sites</button>
  <button onclick="filterMarkers('Transit')" style="padding: 6px 14px; border-radius: 20px; border: 1px solid #8E24AA; background: #F3E5F5; color: #6A1B9A; cursor: pointer; font-weight: bold;">🟣 Transit & Airports</button>
</div>

<div id="full-novascotia-map" style="height: 520px; width: 100%; border-radius: 12px; border: 1px solid #ddd; z-index: 1; margin-bottom: 24px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);"></div>

<script>
var map;
var allMarkers = [];

document.addEventListener("DOMContentLoaded", function() {
    map = L.map('full-novascotia-map').setView([45.8, -62.5], 7);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 18,
        attribution: '© OpenStreetMap contributors'
    }).addTo(map);

    Papa.parse('/assets/data/my_maps_import.csv', {
        download: true,
        header: true,
        complete: function(results) {
            var bounds = [];
            results.data.forEach(function(row) {
                if (row.Lat && row.Lng) {
                    var lat = parseFloat(row.Lat);
                    var lng = parseFloat(row.Lng);
                    if (!isNaN(lat) && !isNaN(lng)) {
                        var markerColor = "#2196F3";
                        if (row.Category === "Base") markerColor = "#E53935";
                        if (row.Category === "Hike") markerColor = "#43A047";
                        if (row.Category === "Activity") markerColor = "#FB8C00";
                        if (row.Category === "Transit") markerColor = "#8E24AA";
                        
                        var customIcon = L.divIcon({
                            className: 'custom-icon',
                            html: `<div style="background-color: ${markerColor}; width: 14px; height: 14px; border-radius: 50%; border: 2px solid white; box-shadow: 0 0 5px rgba(0,0,0,0.6);"></div>`,
                            iconSize: [14, 14],
                            iconAnchor: [7, 7]
                        });
                        var marker = L.marker([lat, lng], {icon: customIcon}).addTo(map);
                        marker.bindPopup(`<b>${row.Name}</b><br><span style="color: #666;">${row.Category} | ${row.Day}</span><br><p style="margin: 4px 0 0;">${row.Description}</p>`);
                        marker.category = row.Category;
                        allMarkers.push(marker);
                        bounds.push([lat, lng]);
                    }
                }
            });
            if(bounds.length > 0) map.fitBounds(bounds, {padding: [30, 30]});
        }
    });
});

function filterMarkers(cat) {
    var bounds = [];
    allMarkers.forEach(function(m) {
        if (cat === 'All' || m.category === cat) {
            map.addLayer(m);
            bounds.push(m.getLatLng());
        } else {
            map.removeLayer(m);
        }
    });
    if(bounds.length > 0) map.fitBounds(bounds, {padding: [30, 30]});
}
</script>

---

## 📍 Master Waypoint & GPS Coordinate Table

| Point of Interest | Category | Day / Leg | Latitude | Longitude | Strategic Function |
|:---|:---:|:---:|:---:|:---:|:---|
| **Halifax Stanfield Airport (YHZ)** | 🟣 Transit | Day 0 / 1 / 6 | 44.8808 | -63.5086 | Main airport & rental car pickup hub |
| **Downtown Halifax Waterfront** | 🔴 Base | Day 0 / 4 / 5 | 44.6476 | -63.5708 | Central accommodation & dining base |
| **Halifax Citadel National Historic Site** | 🟠 Activity | Day 5 | 44.6468 | -63.5813 | Star fortress, noon gun, military history |
| **Alexander Keith's Historic Brewery** | 🟠 Activity | Day 5 | 44.6433 | -63.5728 | 1820 historic brewery tour & tavern |
| **Peggy's Point Lighthouse** | 🟠 Activity | Day 0 / 5 | 44.4930 | -63.9181 | Iconic granite headland & lighthouse |
| **Masstown Market** | 🟠 Activity | Day 1 | 45.3688 | -63.4862 | Route pitstop, bakery, seafood market |
| **Canso Causeway Welcome Pavilion** | 🟣 Transit | Day 1 / 4 | 45.6447 | -61.4178 | Gateway crossing to Cape Breton Island |
| **Baddeck Village** | 🔴 Base | Day 1–3 | 46.1001 | -60.7533 | Bras d'Or Lakes launchpad for Cabot Trail |
| **Alexander Graham Bell NHS** | 🟠 Activity | Day 1 / 4 | 46.1007 | -60.7438 | Bell estate, hydrofoil & Silver Dart exhibits |
| **Uisge Bàn Falls Provincial Park** | 🟢 Hike | Day 4 | 46.1989 | -60.8016 | 2.7 km gorge trail to 16m forest waterfall |
| **Margaree River Salmon Valley** | 🟠 Activity | Day 2 | 46.3314 | -61.0968 | Scenic autumn salmon river & valley |
| **Chéticamp Acadian Village** | 🔴 Base / Hub | Day 2 | 46.6389 | -61.0086 | Acadian cultural center & western park gate |
| **CB Highlands NP - Chéticamp Centre** | 🟣 Transit | Day 2 | 46.6436 | -60.9577 | Discovery Passes & wildlife conditions |
| **French Mountain Lookouts** | 🟠 Activity | Day 2 | 46.7323 | -60.8711 | Coastal switchback cliffside viewpoints |
| **Skyline Trail Trailhead** | 🟢 Hike | Day 2 | 46.7410 | -60.8804 | 6.5 km headland loop & sunset boardwalk |
| **Pleasant Bay** | 🔴 Base / Hub | Day 2 / 3 | 46.8286 | -60.8021 | Whale centre & coastal village |
| **Meat Cove Sea Cliffs** | 🟠 Activity | Day 3 | 47.0264 | -60.5593 | Remote northernmost tip of Cape Breton |
| **Cabot Landing Provincial Park** | 🟠 Activity | Day 3 | 46.9011 | -60.4633 | Historic John Cabot 1497 landfall beach |
| **Neils Harbour Lighthouse & Chowder** | 🟠 Activity | Day 3 | 46.8122 | -60.3236 | Working lobster wharf & chowder shack |
| **Franey Mountain Trailhead** | 🟢 Hike | Day 3 | 46.6669 | -60.4283 | 7.4 km loop with 360° autumn canyon views |
| **Middle Head Trailhead (Keltic Lodge)**| 🟢 Hike | Day 3 | 46.6575 | -60.3802 | 3.8 km ocean peninsula headland trail |
| **Cape Smokey Gondola** | 🟠 Activity | Day 3 | 46.6200 | -60.4100 | Tree-walk tower & summit ocean panorama |
| **The Gaelic College (Colaisde na Gàidhlig)** | 🟠 Activity | Day 4 | 46.2163 | -60.6276 | Great Hall of the Clans & living Gaelic culture |
| **Fortress of Louisbourg NHS** | 🟠 Activity | Day 4 / Extra | 45.8924 | -59.9866 | Massive 18th-century French fortified city |
| **Old Town Lunenburg UNESCO** | 🟠 Activity | Day 0 / Extra | 44.3770 | -64.3090 | 18th-century shipbuilding harbor & Bluenose |
| **Mahone Bay Three Churches** | 🟠 Activity | Day 0 / Extra | 44.4485 | -64.3807 | Scenic harbor town & artisan bakeries |
| **Cape Split Provincial Park** | 🟢 Hike | Day 6 / Extra | 45.3341 | -64.4988 | 13.2 km trail above Fundy tidal whirlpools |
| **Grand-Pré National Historic Site** | 🟠 Activity | Day 6 / Extra | 45.1092 | -64.3106 | UNESCO Acadian dykeland landscape |
| **Luckett Vineyards** | 🟠 Activity | Day 6 / Extra | 45.0686 | -64.3311 | Gaspereau Valley Tidal Bay wine tasting |
| **Burntcoat Head Park** | 🟠 Activity | Day 6 / Extra | 45.3100 | -63.8058 | World's highest recorded tides ocean floor |
