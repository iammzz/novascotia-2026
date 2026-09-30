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
| **Halifax Stanfield Airport (YHZ)** | 🟣 Transit | Day 0 / 1 / 5 | 44.8808 | -63.5086 | Main airport & rental car hub |
| **Moxy Halifax Downtown** | 🔴 Base | Day 0 | 44.6513 | -63.5778 | 🟢 Fri Oct 9 — downtown hotel near Citadel & waterfront |
| **Maritime Museum of the Atlantic** | 🟠 Activity | Day 0 (Fri 6:45 PM) | 44.6493 | -63.5716 | Titanic artifacts & 1917 Halifax Explosion galleries |
| **Halifax Citadel National Historic Site** | 🟠 Activity | Extra | 44.6468 | -63.5813 | Star-shaped 19th-century fortress overlooking the harbour |
| **Martinique Beach Provincial Park** | 🟠 Activity | Day 1 | 44.7190 | -63.1670 | Longest sand beach in Nova Scotia — ocean stretch stop |
| **Taylor Head Provincial Park** | 🟠 Activity | Day 1 | 44.8970 | -62.6110 | Coastal lookout trail over the Atlantic |
| **Sherbrooke Village** | 🟠 Activity | Day 1 | 45.1410 | -61.9870 | Living-history 1860s gold-rush village |
| **Canso Causeway Welcome Pavilion** | 🟣 Transit | Day 1 / 4 | 45.6447 | -61.4178 | Gateway crossing to Cape Breton Island |
| **The Shoreline, West Bay** | 🔴 Base | Day 1 | 45.7133 | -61.1621 | 🟢 Sat Oct 10 — Bras d'Or Lake shoreline retreat |
| **Baddeck Village** | 🟠 Activity | Extra | 46.1001 | -60.7533 | Bras d'Or Lake village — dining & festival detour |
| **Alexander Graham Bell NHS** | 🟠 Activity | Extra | 46.1007 | -60.7438 | Bell estate, hydrofoil & Silver Dart exhibits |
| **Uisge Bàn Falls Provincial Park** | 🟢 Hike | Extra | 46.1989 | -60.8016 | 2.7 km gorge trail to a 16 m forest waterfall |
| **Margaree River Salmon Valley** | 🟠 Activity | Day 2 | 46.3314 | -61.0968 | Scenic autumn salmon river & valley |
| **The Sunset Harbour Village Suite** | 🔴 Base | Day 2 | 46.6317 | -61.0108 | 🟢 Sun Oct 11 — Chéticamp suite 20 min from Skyline |
| **CB Highlands NP — Chéticamp Centre** | 🟣 Transit | Day 2 | 46.6436 | -60.9577 | Discovery Passes & wildlife conditions |
| **French Mountain Lookouts** | 🟠 Activity | Day 2 | 46.7323 | -60.8711 | Coastal switchback cliffside viewpoints |
| **Skyline Trail Trailhead** | 🟢 Hike | Day 2 | 46.7410 | -60.8804 | 6.5 km headland loop & sunset boardwalk |
| **Pleasant Bay** | 🟠 Activity | Day 3 | 46.8286 | -60.8021 | Whale-watching centre & coastal village |
| **Meat Cove Sea Cliffs** | 🟠 Activity | Day 3 | 47.0264 | -60.5593 | Remote northernmost tip of Cape Breton |
| **Cabot Landing Provincial Park** | 🟠 Activity | Day 3 | 46.9011 | -60.4633 | Historic John Cabot 1497 landfall beach |
| **Neils Harbour Lighthouse & Chowder** | 🟠 Activity | Day 3 | 46.8122 | -60.3236 | Working lobster wharf & chowder shack |
| **Franey Mountain Trailhead** | 🟢 Hike | Day 3 | 46.6669 | -60.4283 | 7.4 km loop with 360° autumn canyon views |
| **Middle Head Trailhead (Keltic Lodge)** | 🟢 Hike | Day 3 | 46.6575 | -60.3802 | 3.8 km ocean peninsula headland trail |
| **Keltic Resort at the Highlands** | 🔴 Base | Day 3 | 46.6575 | -60.3802 | 🟢 Mon Oct 12 — cliffside Atlantic resort at Middle Head |
| **Cape Smokey Gondola** | 🟠 Activity | Day 3 | 46.6200 | -60.4100 | Tree-walk tower & summit ocean panorama |
| **The Gaelic College (Colaisde na Gàidhlig)** | 🟠 Activity | Extra | 46.2163 | -60.6276 | Great Hall of the Clans & living Gaelic culture |
| **Fortress of Louisbourg NHS** | 🟠 Activity | Extra | 45.8924 | -59.9866 | Massive 18th-century French fortified city |
| **Grohmann Knives Factory & Outlet** | 🟠 Activity | Day 4 | 45.6765 | -62.7134 | World-famous handcrafted knifemaker & seconds outlet |
| **Hector Heritage Quay** | 🟠 Activity | Day 4 | 45.6753 | -62.7112 | Historic harbour of the Ship Hector |
| **Acropole Pizza (Pictou)** | 🟠 Activity | Day 4 | 45.6764 | -62.7121 | Authentic regional brown-sauce pizza institution |
| **The Lookout Dome One** | 🔴 Base | Day 4 | 44.2758 | -64.3575 | 🟢 Tue Oct 13 — South Shore dome with private hot tub |
| **LaHave Cable Ferry** | 🟣 Transit | Day 5 | 44.2936 | -64.3592 | 5-minute river crossing toward Lunenburg ($7 CAD) |
| **Old Town Lunenburg UNESCO** | 🟠 Activity | Day 5 | 44.3770 | -64.3090 | UNESCO World Heritage shipbuilding harbour & Bluenose |
| **Mahone Bay Three Churches** | 🟠 Activity | Day 5 | 44.4485 | -64.3807 | Scenic harbour town & artisan bakeries |
| **Peggy's Point Lighthouse** | 🟠 Activity | Day 5 | 44.4930 | -63.9181 | Iconic granite headland & lighthouse |
| **Cape Split Provincial Park** | 🟢 Hike | Extra | 45.3341 | -64.4988 | 13.2 km trail above Fundy tidal whirlpools |
| **Grand-Pré National Historic Site** | 🟠 Activity | Extra | 45.1092 | -64.3106 | UNESCO Acadian dykeland landscape |
| **Luckett Vineyards** | 🟠 Activity | Extra | 45.0686 | -64.3311 | Gaspereau Valley Tidal Bay wine tasting |
| **Burntcoat Head Park** | 🟠 Activity | Extra | 45.3100 | -63.8058 | World's highest recorded tides ocean floor |

---

## 🚗 Master Daily Turn-by-Turn Google Maps Navigation Routes

Click any link below to launch sequential turn-by-turn driving directions pre-loaded directly in Google Maps for live GPS navigation on your phone or in-car display:

| Day | Key Routing Stops | Drive | Google Maps Route Link |
|:---|:---|:---:|:---:|
| **Day 0 · Fri Oct 9** — Arrival | YHZ Airport → Moxy Halifax Downtown → Maritime Museum | ~38 km (40 m) | [**🔗 Launch Day 0**](https://www.google.com/maps/dir/Halifax+Stanfield+International+Airport+%28YHZ%29%2C+Enfield%2C+NS/5417+Cogswell+St,+Halifax,+NS+B3J+1R1/Maritime+Museum+of+the+Atlantic,+Lower+Water+Street,+Halifax,+NS){:target="_blank"} |
| **Day 1 · Sat Oct 10** — Marine Drive to West Bay | Halifax Train Station → Martinique Beach → Tangier → Taylor Head → Sherbrooke Village → Guysborough → Canso Causeway → West Bay | ~380 km (6 h) | [**🔗 Launch Day 1**](https://www.google.com/maps/dir/Halifax+Train+Station%2C+1161+Hollis+Street%2C+Halifax%2C+NS/Musquodoboit+Harbour%2C+NS/Martinique+Beach+Provincial+Park%2C+NS/Tangier%2C+NS/Sheet+Harbour%2C+NS/Taylor+Head+Provincial+Park%2C+NS/Sherbrooke+Village%2C+Sherbrooke%2C+NS/Guysborough%2C+NS/Canso+Causeway+Visitor+Information+Centre%2C+Port+Hastings%2C+NS/108+Camerons+Road%2C+West+Bay%2C+NS){:target="_blank"} |
| **Day 2 · Sun Oct 11** — Margaree & Skyline Sunset | West Bay → Margaree Valley → Chéticamp → French Mtn → Skyline Trail → The Sunset Harbour Village Suite | ~170 km (2 h 45 m) | [**🔗 Launch Day 2**](https://www.google.com/maps/dir/108+Camerons+Road,+West+Bay,+NS/Margaree+Forks,+NS/Les+Trois+Pignons,+Cabot+Trail,+Ch%C3%A9ticamp,+NS/Cap+Rouge+Lookout,+Cabot+Trail,+Ch%C3%A9ticamp,+NS/Skyline+Trail+Trailhead,+Cabot+Trail,+Cape+Breton+Highlands+National+Park,+NS/15294+Cabot+Trail,+Ch%C3%A9ticamp,+NS){:target="_blank"} |
| **Day 3 · Mon Oct 12** — Highlands, Meat Cove & Keltic | Chéticamp → North Mtn → Meat Cove → Neils Harbour → Franey Trail → Cape Smokey → **Keltic Resort, Ingonish** | ~190 km (3 h 15 m) | [**🔗 Launch Day 3**](https://www.google.com/maps/dir/15294+Cabot+Trail,+Ch%C3%A9ticamp,+NS/MacKenzie+Mountain+Lookout,+Cabot+Trail,+NS/Meat+Cove+Chowder+Hut,+Meat+Cove+Road,+Meat+Cove,+NS/Neils+Harbour+Lighthouse,+Neils+Harbour,+NS/Franey+Trailhead,+Franey+Road,+Ingonish+Centre,+NS/Cape+Smokey+Gondola,+Cabot+Trail,+Ingonish+Ferry,+NS/Keltic+Lodge+at+the+Highlands,+Ingonish+Beach,+NS){:target="_blank"} |
| **Day 4 · Tue Oct 13** — Pictou & Grohmann to LaHave | **Keltic Resort (Ingonish)** → Canso Causeway → Grohmann Knives (Pictou) → Pictou Waterfront → **The Lookout Dome One (LaHave)** | ~560 km (5 h 45 m) | [**🔗 Launch Day 4**](https://www.google.com/maps/dir/Keltic+Lodge+at+the+Highlands,+Ingonish+Beach,+NS/Canso+Causeway+Visitor+Information+Centre,+Port+Hastings,+NS/Grohmann+Knives+Ltd,+Water+Street,+Pictou,+NS/3839+Nova+Scotia+331,+LaHave,+NS){:target="_blank"} |
| **Day 5 · Wed Oct 14** — South Shore & Evening Flight | **The Lookout Dome (LaHave)** → LaHave Cable Ferry → Lunenburg UNESCO → Mahone Bay → Peggy's Point Lighthouse → **YHZ Airport** | ~190 km (3 h 30 m) | [**🔗 Launch Day 5**](https://www.google.com/maps/dir/3839+Nova+Scotia+331,+LaHave,+NS/Old+Town+Lunenburg,+Montague+Street,+Lunenburg,+NS/Mahone+Bay,+NS/Peggy's+Point+Lighthouse,+Peggy's+Point+Road,+Peggy's+Cove,+NS/Halifax+Stanfield+International+Airport+(YHZ),+Enfield,+NS){:target="_blank"} |
