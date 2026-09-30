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

!!! info "📋 Booking Status: 🟢 4 OF 5 NIGHTS BOOKED (80% Accommodation Locked)"
    **Core Activity Dates Locked**: **October 10 – 14, 2026** (Canadian Thanksgiving Peak Foliage Window)  
    **Return to Toronto**: **Day 5 (Wed, Oct 14) evening flight — WestJet WS811 20:15, BOOKED** — departing YHZ after exploring LaHave, Lunenburg UNESCO Old Town, Mahone Bay, and Peggy's Cove. Day 6 retired.  
    **Confirmed Bookings**: 🟢 **Flights (Porter PD201 out / WestJet WS811 return)** · 🟢 **Rental Car (National #2098647349)** · 🟢 **Skyline Parking (Sun Oct 11, 4 PM)** · 🟢 **4 of 5 Lodging Nights (Moxy Halifax Oct 9, Chéticamp Oct 11, Keltic Ingonish Oct 12, The Lookout Dome LaHave Oct 13)**  
    **Only Remaining Critical Action**: Book **Saturday, Oct 10 night in Baddeck** before Celtic Colours festival sellout!

---

## 🧭 Trip Master Overview

### 🛫 Travel Buffer & Arrival Window
| Day | Date | Primary Focus & Highlights | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 0**](day0_flight_transit_arrival.md) | **Fri, Oct 9** | ✈️ **Arrival locked**: morning Toronto → Halifax flight (PD201 lands 11:37 AM), check into **Moxy Halifax Downtown (BOOKED)**, Maritime Museum of the Atlantic evening reservation | Moxy Halifax Downtown | ~38 km (taxi) | ⭐ |

---

### 🍁 Core Activity Days (The Must-Do Window: Oct 10–14)
| Day | Date | Primary Focus & Key Activities | Base Camp | Drive / Time | Intensity |
|:---|:---|:---|:---|:---|:---:|
| [**Day 1**](day1_halifax_to_baddeck.md) | **Sat, Oct 10** | **Marine Drive (Route 7, Eastern Shore)**: Martinique Beach, Taylor Head lookout, Sherbrooke Village, Guysborough, Canso Causeway → Baddeck + Celtic Colours ceilidh | Baddeck (⚠️ Unbooked) | ~405 km (~5h) | ⭐⭐ |
| [**Day 2**](day2_western_cabot_trail_skyline.md) | **Sun, Oct 11** | Western Cabot Trail, Margaree Valley, Chéticamp Acadian culture & **Skyline Trail Sunset Hike (4 PM booked)** → sleep in Chéticamp | Chéticamp (Suite BOOKED) | ~145 km (~2.5h) | ⭐⭐⭐ |
| [**Day 3**](day3_northern_eastern_cabot_trail.md) | **Mon, Oct 12** | Northern Highlands, Meat Cove Sea Cliffs, Neils Harbour, **Franey Mountain Trail** & Cape Smokey — **night at the Keltic Resort Ingonish (BOOKED)** | Ingonish (Keltic BOOKED) | ~190 km (~3.2h) | ⭐⭐⭐⭐ |
| [**Day 4**](day4_baddeck_to_halifax_heritage.md) | **Tue, Oct 13** | Check out Keltic Ingonish → **Pictou Waterfront & Grohmann Knives Factory Tour** → drive to South Shore → **The Lookout Dome One with private hot tub (BOOKED)** | LaHave (Dome BOOKED) | ~560 km (~5.8h) | ⭐⭐ |
| [**Day 5**](day5_halifax_peggys_cove.md) | **Wed, Oct 14** | 🌊 **LaHave Bakery & Cable Ferry → Lunenburg UNESCO Old Town → Mahone Bay → Peggy's Cove** & **evening return flight (WestJet WS811, 20:15 BOOKED)** | Airport Transit | ~150 km (~2.5h) | ⭐⭐ |

---


