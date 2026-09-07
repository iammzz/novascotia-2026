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
| **Halifax Citadel National Historic Site** | 🟠 Activity | Extra (not Day 5) | 44.6468 | -63.5813 | Star fortress perimeter + noon gun — cut from Day 5 (South Shore loop); interior exhibits cut |
| **Maritime Museum of the Atlantic** | 🟠 Activity | **Day 0 (Fri 6:45 PM, reserved)** | 44.6493 | -63.5716 | Titanic wooden artifacts & 1917 Halifax Explosion galleries |
| **Peggy's Point Lighthouse** | 🟠 Activity | Day 5 | 44.4930 | -63.9181 | Iconic granite headland & lighthouse |
| **Martinique Beach Provincial Park** | 🟠 Activity | Day 1 | 44.7190 | -63.1670 | Longest sand beach in Nova Scotia — ocean stretch stop |
| **Taylor Head Provincial Park** | 🟠 Activity | Day 1 | 44.8970 | -62.6110 | Coastal lookout trail over the Atlantic (signature Marine Drive viewpoint) |
| **Sherbrooke Village** | 🟠 Activity | Day 1 | 45.1410 | -61.9870 | Living-history 1860s gold-rush village (Nova Scotia Museum) |
| **Canso Causeway Welcome Pavilion** | 🟣 Transit | Day 1 / 4 | 45.6447 | -61.4178 | Gateway crossing to Cape Breton Island |
| **Baddeck Village** | 🔴 Base | Day 1–3 | 46.1001 | -60.7533 | Bras d'Or Lakes launchpad for Cabot Trail |
| **Alexander Graham Bell NHS** | 🟠 Activity | Extra | 46.1007 | -60.7438 | Bell estate, hydrofoil & Silver Dart exhibits (cut from Day 1 for Marine Drive) |
| **Uisge Bàn Falls Provincial Park** | 🟢 Hike | Extra | 46.1989 | -60.8016 | 2.7 km gorge trail to 16m forest waterfall (cut from Day 4 — Ingonish-start route)
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
| **The Gaelic College (Colaisde na Gàidhlig)** | 🟠 Activity | Extra | 46.2163 | -60.6276 | Great Hall of the Clans & living Gaelic culture (cut from Day 4 — Ingonish-start route)
| **Fortress of Louisbourg NHS** | 🟠 Activity | Day 4 / Extra | 45.8924 | -59.9866 | Massive 18th-century French fortified city |
| **Old Town Lunenburg UNESCO** | 🟠 Activity | Day 5 | 44.3770 | -64.3090 | UNESCO World Heritage shipbuilding harbor & Bluenose story |
| **Mahone Bay Three Churches** | 🟠 Activity | Day 5 | 44.4485 | -64.3807 | Scenic harbor town & artisan bakeries |
| **Cape Split Provincial Park** | 🟢 Hike | Day 6 / Extra | 45.3341 | -64.4988 | 13.2 km trail above Fundy tidal whirlpools |
| **Grand-Pré National Historic Site** | 🟠 Activity | Day 6 / Extra | 45.1092 | -64.3106 | UNESCO Acadian dykeland landscape |
| **Luckett Vineyards** | 🟠 Activity | Day 6 / Extra | 45.0686 | -64.3311 | Gaspereau Valley Tidal Bay wine tasting |
| **Burntcoat Head Park** | 🟠 Activity | Day 6 / Extra | 45.3100 | -63.8058 | World's highest recorded tides ocean floor |
| **Grohmann Knives Factory & Outlet** | 🟠 Activity | Day 4 | 45.6765 | -62.7134 | World-famous handcrafted knifemaker & seconds outlet |
| **Hector Heritage Quay** | 🟠 Activity | Day 4 | 45.6753 | -62.7112 | Historic harbor of Ship Hector (Birthplace of New Scotland) |
| **Acropole Pizza (Pictou)** | 🟠 Activity | Day 4 | 45.6764 | -62.7121 | Authentic regional brown-sauce pizza institution |

---

## 🚗 Master Daily Turn-by-Turn Google Maps Navigation Routes

Click any link below to launch sequential turn-by-turn driving directions pre-loaded directly in Google Maps for live GPS navigation on your phone or in-car display:

| Day & Title | Key Routing Stops | Total Drive & Dist | Google Maps Route Link |
|:---|:---|:---:|:---:|
| **Day 0 (Arrival, Fri Oct 9, locked)**: Airport → Hotel → Maritime Museum | YHZ Airport → Downtown Halifax Waterfront → Maritime Museum of the Atlantic | ~39 km (45m) | [**👉 Launch Arrival Route**](https://www.google.com/maps/dir/Halifax+Stanfield+International+Airport+%28YHZ%29%2C+Enfield%2C+NS/Queen%27s+Marque%2C+Lower+Water+Street%2C+Halifax%2C+NS/Maritime+Museum+of+the+Atlantic%2C+Lower+Water+Street%2C+Halifax%2C+NS){:target="_blank"} |
| ~~**Day 0 (Option B)**: South Shore UNESCO Loop~~ | ~~YHZ → Lunenburg → Mahone Bay → Halifax~~ | ~~~205 km (2h 30m)~~ | ❌ Retired (Lunenburg/Mahone Bay moved to optional 'Extra' list) |
| **Day 1**: Marine Drive (Eastern Shore) to Baddeck | Halifax Train Station → Martinique Beach → Tangier → Taylor Head Lookout → Sherbrooke Village → Guysborough → Canso Causeway → Baddeck | ~405 km (5h 00m) | [**👉 Launch Day 1 (Marine Drive) Route**](https://www.google.com/maps/dir/Halifax+Train+Station%2C+1161+Hollis+Street%2C+Halifax%2C+NS/Musquodoboit+Harbour%2C+NS/Martinique+Beach+Provincial+Park%2C+NS/Tangier%2C+NS/Sheet+Harbour%2C+NS/Taylor+Head+Provincial+Park%2C+NS/Sherbrooke+Village%2C+Sherbrooke%2C+NS/Guysborough%2C+NS/Canso+Causeway+Visitor+Information+Centre%2C+Port+Hastings%2C+NS/Baddeck%2C+NS){:target="_blank"} |
| **Day 2**: Western Cabot Trail & Skyline Sunset | Baddeck → Margaree Valley → Chéticamp → French Mtn → Skyline Trail → Doryman | ~145 km (2h 15m) | [**👉 Launch Day 2 Route**](https://www.google.com/maps/dir/Baddeck%2C+NS/Margaree+Forks%2C+NS/Les+Trois+Pignons%2C+Cabot+Trail%2C+Ch%C3%A9ticamp%2C+NS/Cap+Rouge+Lookout%2C+Cabot+Trail%2C+Ch%C3%A9ticamp%2C+NS/Skyline+Trail+Trailhead%2C+Cabot+Trail%2C+Cape+Breton+Highlands+National+Park%2C+NS/The+Doryman+Pub+%26+Grill%2C+Cabot+Trail%2C+Ch%C3%A9ticamp%2C+NS){:target="_blank"} |
| **Day 3**: Highlands, Meat Cove & Keltic Ingonish (BOOKED) | Chéticamp → North Mtn → Meat Cove → Neils Harbour → Franey Trail → Smokey Gondola → **Keltic Resort, Ingonish** | ~190 km (3h 15m) | [**👉 Launch Day 3 Route**](https://www.google.com/maps/dir/Ch%C3%A9ticamp%2C+NS/MacKenzie+Mountain+Lookout%2C+Cabot+Trail%2C+NS/Meat+Cove+Chowder+Hut%2C+Meat+Cove+Road%2C+Meat+Cove%2C+NS/Neils+Harbour+Lighthouse%2C+Neils+Harbour%2C+NS/Franey+Trailhead%2C+Franey+Road%2C+Ingonish+Centre%2C+NS/Cape+Smokey+Gondola%2C+Cabot+Trail%2C+Ingonish+Ferry%2C+NS/Keltic+Lodge+at+the+Highlands%2C+Ingonish+Beach%2C+NS){:target="_blank"} |
| **Day 4**: Pictou & Grohmann from Ingonish | **Keltic Resort (Ingonish)** → Canso Causeway → Grohmann Knives → Pictou Waterfront → Halifax | ~465 km (5h 15m) | [**👉 Launch Day 4 Route**](https://www.google.com/maps/dir/Keltic+Lodge+at+the+Highlands%2C+Ingonish+Beach%2C+NS/Canso+Causeway+Visitor+Information+Centre%2C+Port+Hastings%2C+NS/Grohmann+Knives+Ltd%2C+Water+Street%2C+Pictou%2C+NS/Hector+Heritage+Quay%2C+Caladh+Avenue%2C+Pictou%2C+NS/Downtown+Halifax%2C+Halifax%2C+NS){:target="_blank"} |
| **Day 5** (Return, Wed Oct 14): South Shore Loop & Evening Flight (WS811 20:15) | Halifax → Peggy's Cove → Mahone Bay → Lunenburg Old Town → **YHZ Airport** | ~240 km (3h 20m) | [**👉 Launch Day 5 Route**](https://www.google.com/maps/dir/Downtown+Halifax%2C+Halifax%2C+NS/Peggy%27s+Point+Lighthouse%2C+Peggy%27s+Point+Road%2C+Peggy%27s+Cove%2C+NS/Mahone+Bay%2C+NS/Old+Town+Lunenburg%2C+Montague+Street%2C+Lunenburg%2C+NS/Halifax+Stanfield+International+Airport+%28YHZ%29%2C+Enfield%2C+NS){:target="_blank"} |
| **Day 6** (⚠️ Optional — only if return pushed to Oct 15/16): Bay of Fundy, Wine & YHZ | Halifax → UNESCO Grand-Pré → Cape Split Hike → Hall's Harbour → Luckett Vineyards → YHZ Airport | ~245 km (3h 25m) | [**👉 Launch Day 6 Route**](https://www.google.com/maps/dir/Downtown+Halifax%2C+Halifax%2C+NS/Grand-Pr%C3%A9+National+Historic+Site%2C+Grand-Pr%C3%A9+Road%2C+Grand-Pr%C3%A9%2C+NS/Cape+Split+Trailhead%2C+Scots+Bay+Road%2C+Scots+Bay%2C+NS/The+Lobster+Pound+%26+Restaurant+at+Hall%27s+Harbour%2C+West+Halls+Harbour+Road%2C+Halls+Harbour%2C+NS/Luckett+Vineyards%2C+Grand+Pr%C3%A9+Road%2C+Wolfville%2C+NS/Halifax+Stanfield+International+Airport+%28YHZ%29%2C+Enfield%2C+NS){:target="_blank"} |

