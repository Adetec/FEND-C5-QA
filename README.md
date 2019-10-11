# FEND-C5 QA

## Restaurant reviews project

### Q1. My map doesn't show up
I created my mapbox token and input it on both `main.js` & `restaurant_info.js`, but I got a 401 error msg

```
/**
 * Initialize leaflet map, called from HTML.
 */
initMap = () => {
  self.newMap = L.map('map', {
        center: [40.722216, -73.987501],
        zoom: 12,
        scrollWheelZoom: false
      });
  L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.jpg70?access_token={mapboxToken}', {
    mapboxToken: '<pk.eyJ1IjoiYWRldGVjaCIsImEiOiJjamtpdjM5YnkxOWt2M3JreGpkbnl6N3M3In0.6nZz0MSvsz6HKHJfYq-0zg>',
    maxZoom: 18,
    attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
      '<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ' +
      'Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
    id: 'mapbox.streets'
  }).addTo(newMap);

  updateRestaurants();
}

```
![](img/mapbox-token.png?raw=true)

How can I fix that?

### Answer
Simply, you did forget to remove open and close tags at the beginning and the end of your token
```
mapboxToken: 'pk.eyJ1IjoiYWRldGVjaCIsImEiOiJjamtpdjM5YnkxOWt2M3JreGpkbnl6N3M3In0.6nZz0MSvsz6HKHJfYq-0zg',
```

It should be working now
***





