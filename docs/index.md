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

!!! info "📋 Booking Status: 🟡 PARTIALLY BOOKED"
    **Core Activity Dates Locked**: **October 10 – 14, 2026** (Canadian Thanksgiving Peak Foliage Window)  
    **Return to Toronto**: **Day 5 (Wed, Oct 14) evening flight — WestJet WS811 20:15, BOOKED** — the day is a Peggy's Cove sunrise + Citadel noon gun + farewell lunch, then home. Day 6 (Oct 15/16) retired.  
    **Confirmed Bookings**: 🟢 **Flights — Porter PD201 out (Fri Oct 9, 8:30 AM) + WestJet WS811 return (Wed Oct 14, 20:15)** · 🟢 **Rental Car — National #2098647349** · 🟢 **Skyline Trail Parking (Sun Oct 11, 4 PM)**  
    **Current Next Priorities**: Book Halifax hotels (Oct 9 & 13) + Cape Breton lodgings (Oct 10–12) — then change the rental return to ~6:00 PM (not 7:30 PM) and confirm Maritime Museum evening entry.

---

## 📅 Trip Master Overview

### 🛫 Travel Buffer & Arrival Window
| Day | Date | Primary Focus & Highlights | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 0**](day0_flight_transit_arrival.md) | **Fri, Oct 9** | ✈️ **Arrival locked**: morning Toronto → Halifax flight, hotel check-in, Maritime Museum of the Atlantic evening reservation | Halifax Waterfront | ~40 km (taxi) | ⭐ |

---

### 🍁 Core Activity Days (The Must-Do Window: Oct 10–14)
| Day | Date | Primary Focus & Key Activities | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 1**](day1_halifax_to_baddeck.md) | **Sat, Oct 10** | Halifax to Cape Breton: Masstown Market, Canso Causeway, Alexander Graham Bell NHS, Bras d'Or Lakes | Baddeck | ~355 km (~4h) | ⭐⭐ |
| [**Day 2**](day2_western_cabot_trail_skyline.md) | **Sun, Oct 11** | Western Cabot Trail, Margaree Valley, Chéticamp Acadian culture & **Skyline Trail Sunset Hike** | Chéticamp / Baddeck | ~145 km (~2.5h) | ⭐⭐⭐ |
| [**Day 3**](day3_northern_eastern_cabot_trail.md) | **Mon, Oct 12** | Northern Highlands, Meat Cove Sea Cliffs, Neils Harbour, **Franey Mountain Trail** & Cape Smokey | Ingonish / Baddeck | ~180 km (~3h) | ⭐⭐⭐⭐ |
| [**Day 4**](day4_baddeck_to_halifax_heritage.md) | **Tue, Oct 13** | **Uisge Bàn Falls Hike**, The Gaelic College, **Pictou Waterfront & Grohmann Knives Factory Tour**, evening Halifax North End | Downtown Halifax | ~439 km (~4.5h) | ⭐⭐ |
| [**Day 5**](day5_halifax_peggys_cove.md) | **Wed, Oct 14** | 🌅 **Peggy's Cove sunrise**, Citadel noon gun, farewell lunch & **evening return flight (WestJet WS811, 20:15)** (Maritime Museum already done Fri) | Halifax → YHZ | ~150 km (~2.5h) | ⭐⭐ |

---

### 🛬 Travel Buffer & Extension Window
| Day | Date | Primary Focus & Highlights | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 6**](day6_bay_of_fundy_departure.md) | Oct 15 / 16 | ⚠️ Optional extension (only if return flight pushed back): **Bay of Fundy Cape Split Hike** OR Annapolis Valley Wine Country (Luckett / Grand-Pré), then fly home | Airport Transit | ~190 km (~2.5h) | ⭐⭐⭐ |

---

## 📌 Fast Navigation

- 📋 **[Active To-Do Checklist](todo.md)**: Phased task manager and booking timeline.
- ✈️ **[Logistics Master](logistics.md)**: Flight options, rental vehicle details, and accommodation hubs.
- 🔍 **[Flight Research & Value Matrix](research.md)**: Detailed Toronto (YYZ/YTZ) to Halifax (YHZ) airline analysis and date sensitivity matrix.
- 🧳 **[Master Packing List](packing_list.md)**: Autumn hiking gear, weather protection, headlamps, and pass requirements.
- 🗺️ **[Interactive GPS Map](maps.md)**: Fullscreen filterable Leaflet map with all 30+ waypoints.
- 💡 **[Potential Activities Vault](potential_activities.md)**: Back-pocket adventures, ocean activities, and cultural experiences.
- 🧭 **[Ideas for Extra Days](ideas_for_extra.md)**: Prince Edward Island, South Shore deep dive, and tidal bore rafting.
