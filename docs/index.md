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

!!! info "📋 Booking Status: 🟢 FULLY BOOKED — 5 of 5 Nights Confirmed"
    **Core Itinerary Dates**: **October 9 – 14, 2026 (6 Days)**  
    **Flights**: Porter PD201 out (Fri Oct 9, 8:30 AM) · WestJet WS811 return (Wed Oct 14, 20:15)  
    **Ground & Activities**: National Car Rental booked · Skyline Trail sunset parking booked (Sun Oct 11, 4 PM)  
    **Accommodations**: All 5 nights confirmed — Moxy Halifax (Oct 9), The Shoreline West Bay (Oct 10), The Sunset Harbour Village Suite Chéticamp (Oct 11), Keltic Resort Ingonish (Oct 12), The Lookout Dome LaHave (Oct 13).

---

## 🧭 Core Itinerary Overview (October 9–14, 2026)

| Day | Date | Primary Focus & Highlights | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 0**](day0_flight_transit_arrival.md) | **Fri, Oct 9** | ✈️ Morning flight from Toronto (PD201 lands 11:37 AM), check into **Moxy Halifax Downtown (BOOKED)**, visit the **Maritime Museum of the Atlantic** (Titanic & 1917 Explosion galleries) | Moxy Halifax Downtown | ~38 km (taxi) | ⭐ |
| [**Day 1**](day1_halifax_to_west_bay.md) | **Sat, Oct 10** | **Marine Drive (Route 7, Eastern Shore)**: Pick up rental car 9 AM, Martinique Beach, Taylor Head coastal hike, Sherbrooke Village, Canso Causeway → lakeside base on the Bras d'Or Lake | West Bay (BOOKED) | ~380 km (~6h) | ⭐⭐ |
| [**Day 2**](day2_western_cabot_trail_skyline.md) | **Sun, Oct 11** | Drive the **Margaree Valley**, explore **Chéticamp** Acadian culture & hike the **Skyline Trail at sunset (4 PM booked)** → sleep 20 min from the trailhead | Chéticamp (BOOKED) | ~170 km (~2.75h) | ⭐⭐⭐ |
| [**Day 3**](day3_northern_eastern_cabot_trail.md) | **Mon, Oct 12** | Northern Highlands, Meat Cove Sea Cliffs, Neils Harbour, **Franey Mountain Trail** & Cape Smokey — **night at the Keltic Resort Ingonish (BOOKED)** | Ingonish (Keltic BOOKED) | ~190 km (~3.2h) | ⭐⭐⭐⭐ |
| [**Day 4**](day4_ingonish_to_lahave.md) | **Tue, Oct 13** | Check out Keltic Ingonish → **Pictou Waterfront & Grohmann Knives Factory Tour** → drive to the South Shore → **The Lookout Dome with private hot tub (BOOKED)** | LaHave (BOOKED) | ~560 km (~5.75h) | ⭐⭐ |
| [**Day 5**](day5_lahave_to_yhz.md) | **Wed, Oct 14** | 🌊 **LaHave Bakery & Cable Ferry → Lunenburg UNESCO Old Town → Mahone Bay → Peggy's Cove** & **evening return flight (WestJet WS811, 20:15 BOOKED)** | Return to Toronto | ~190 km (~3.5h) | ⭐⭐ |

---

## ⚡ Fast Navigation

- 📋 **[Active To-Do Checklist](todo.md)**: Action task manager and booking timeline.
- ✈️ **[Logistics Master](logistics.md)**: Flight details, rental vehicle specifications, and accommodation hubs.
- 📊 **[Flight Research & Value Matrix](research.md)**: Detailed Toronto (YYZ) to Halifax (YHZ) airline analysis.
- 🎒 **[Master Packing List](packing_list.md)**: Autumn hiking gear, weather protection, headlamps, and pass requirements.
- 🗺️ **[Interactive GPS Map](maps.md)**: Fullscreen filterable Leaflet map with all waypoints.
- 💡 **[Potential Activities Vault](potential_activities.md)**: Back-pocket adventures, ocean activities, and cultural experiences.
- 🧭 **[Ideas for Extra Days](ideas_for_extra.md)**: Prince Edward Island, Annapolis Valley wine country, and tidal bore rafting.
