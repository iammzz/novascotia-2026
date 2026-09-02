# 🍁 Nova Scotia 2026: Autumn Road Trip & Cabot Trail Master Plan

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>

<div id="novascotia-map" style="height: 380px; width: 100%; border-radius: 12px; border: 1px solid #ddd; z-index: 1; margin-bottom: 24px; box-shadow: 0 2px 8px rgba(0,0,0,0.08);"></div>

<script>
document.addEventListener("DOMContentLoaded", function() {
    var map = L.map('novascotia-map').setView([45.8, -62.5], 7);
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
                        var markerColor = "#2196F3"; // Blue default
                        if (row.Category === "Base") markerColor = "#E53935"; // Red
                        if (row.Category === "Hike") markerColor = "#43A047"; // Green
                        if (row.Category === "Activity") markerColor = "#FB8C00"; // Orange
                        if (row.Category === "Transit") markerColor = "#8E24AA"; // Purple
                        
                        var customIcon = L.divIcon({
                            className: 'custom-icon',
                            html: `<div style="background-color: ${markerColor}; width: 14px; height: 14px; border-radius: 50%; border: 2px solid white; box-shadow: 0 0 5px rgba(0,0,0,0.6);"></div>`,
                            iconSize: [14, 14],
                            iconAnchor: [7, 7]
                        });
                        var marker = L.marker([lat, lng], {icon: customIcon}).addTo(map);
                        marker.bindPopup(`<b>${row.Name}</b><br><span style="color: #666;">${row.Category} | ${row.Day}</span><br>${row.Description}`);
                        bounds.push([lat, lng]);
                    }
                }
            });
            if(bounds.length > 0) map.fitBounds(bounds, {padding: [25, 25]});
        }
    });
});
</script>

!!! warning "🚨 Status: Planning & Research Phase"
    **Core Activity Dates Locked**: **October 10 – 14, 2026** (Canadian Thanksgiving Peak Foliage Window)  
    **Buffer Windows**: Potential early arrival (Oct 8/9) and late departure (Oct 15/16) pending flight price optimization.  
    **Current Action Items**: Review Toronto–Halifax flight value matrix in [Flight Research](research.md) & lock in rental car + Cape Breton lodgings (see [To-Do List](todo.md)).

---

## 📅 Trip Master Overview

### 🛫 Travel Buffer & Arrival Window
| Day | Date | Primary Focus & Highlights | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 0**](day0_flight_transit_arrival.md) | Oct 8 / 9 | Fly Toronto (YYZ/YTZ) to Halifax (YHZ), pick up car, Halifax Waterfront or UNESCO Lunenburg | Halifax Waterfront | ~35–110 km | ⭐ |

---

### 🍁 Core Activity Days (The Must-Do Window: Oct 10–14)
| Day | Date | Primary Focus & Key Activities | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 1**](day1_halifax_to_baddeck.md) | **Sat, Oct 10** | Halifax to Cape Breton: Masstown Market, Canso Causeway, Alexander Graham Bell NHS, Bras d'Or Lakes | Baddeck | ~355 km (~4h) | ⭐⭐ |
| [**Day 2**](day2_western_cabot_trail_skyline.md) | **Sun, Oct 11** | Western Cabot Trail, Margaree Valley, Chéticamp Acadian culture & **Skyline Trail Sunset Hike** | Chéticamp / Baddeck | ~145 km (~2.5h) | ⭐⭐⭐ |
| [**Day 3**](day3_northern_eastern_cabot_trail.md) | **Mon, Oct 12** | Northern Highlands, Meat Cove Sea Cliffs, Neils Harbour, **Franey Mountain Trail** & Cape Smokey | Ingonish / Baddeck | ~180 km (~3h) | ⭐⭐⭐⭐ |
| [**Day 4**](day4_baddeck_to_halifax_heritage.md) | **Tue, Oct 13** | **Uisge Bàn Falls Hike**, The Gaelic College at St. Ann's, scenic Bras d'Or drive to Halifax | Downtown Halifax | ~365 km (~4h) | ⭐⭐ |
| [**Day 5**](day5_halifax_peggys_cove.md) | **Wed, Oct 14** | **Peggy's Point Lighthouse**, Halifax Citadel Noon Gun, Waterfront Boardwalk & Alexander Keith's Brewery | Downtown Halifax | ~95 km (~1.5h) | ⭐⭐ |

---

### 🛬 Travel Buffer & Extension Window
| Day | Date | Primary Focus & Highlights | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 6**](day6_bay_of_fundy_departure.md) | Oct 15 / 16 | **Bay of Fundy Cape Split Hike** OR Annapolis Valley Wine Country (Luckett / Grand-Pré), fly home to Toronto | Airport Transit | ~190 km (~2.5h) | ⭐⭐⭐ |

---

## 📌 Fast Navigation

- 📋 **[Active To-Do Checklist](todo.md)**: Phased task manager and booking timeline.
- ✈️ **[Logistics Master](logistics.md)**: Flight options, rental vehicle details, and accommodation hubs.
- 🔍 **[Flight Research & Value Matrix](research.md)**: Detailed Toronto (YYZ/YTZ) to Halifax (YHZ) airline analysis and date sensitivity matrix.
- 🧳 **[Master Packing List](packing_list.md)**: Autumn hiking gear, weather protection, headlamps, and pass requirements.
- 🗺️ **[Interactive GPS Map](maps.md)**: Fullscreen filterable Leaflet map with all 30+ waypoints.
- 💡 **[Potential Activities Vault](potential_activities.md)**: Back-pocket adventures, ocean activities, and cultural experiences.
- 🧭 **[Ideas for Extra Days](ideas_for_extra.md)**: Prince Edward Island, South Shore deep dive, and tidal bore rafting.
