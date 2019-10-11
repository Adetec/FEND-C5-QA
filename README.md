
- [Restaurant reviews project](#restaurant-reviews-project)
  - [Q1. My map doesn't show up](#q1-my-map-doesnt-show-up)
    - [Answer](#answer)
  - [Q2. Problem with python server](#q2-problem-with-python-server)
    - [Answer](#answer-1)
  - [Q3. Responsive design](#q3-responsive-design)
    - [Answer](#answer-2)
  - [Q4. Accessibility](#q4-accessibility)
    - [Answer](#answer-3)
  - [Q5. Service worker](#q5-service-worker)
    - [Answer](#answer-4)
## Restaurant reviews project

### Q1. My map doesn't show up
I created my mapbox token and input it on both `main.js` & `restaurant_info.js`, but I got a 401 error msg

```JS
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

#### Answer
Simply, you did forget to remove open and close tags at the beginning and the end of your token
```JS
mapboxToken: 'pk.eyJ1IjoiYWRldGVjaCIsImEiOiJjamtpdjM5YnkxOWt2M3JreGpkbnl6N3M3In0.6nZz0MSvsz6HKHJfYq-0zg',
```

It should be working now
***

### Q2. Problem with python server

When I run the following command lin on my terminal to start the server
```bash
python3 -m http.server 8000
```

I get bunch of error lines, it says: **Address already in use**
```bash
python3 -m http.server 8000
Traceback (most recent call last):
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/http/server.py", line 1262, in <module>
    test(HandlerClass=handler_class, port=args.port, bind=args.bind)
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/http/server.py", line 1230, in test
    with ServerClass(server_address, HandlerClass) as httpd:
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/socketserver.py", line 452, in __init__
    self.server_bind()
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/http/server.py", line 137, in server_bind
    socketserver.TCPServer.server_bind(self)
  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/socketserver.py", line 466, in server_bind
    self.socket.bind(self.server_address)
OSError: [Errno 48] Address already in use
```
#### Answer

probably you have a server already running on your machine with the same port 8000, perhaps there is a process running in background and using that port, so you need to change your app port ton another number:

On `db_helper.js` file of your project, change the port value

* e.g: ```const port= 8060```
* Try to run the server with updated port
```
python3 -m http.server 8060
```
* Don't forget to update your `README.md` file
***

### Q3. Responsive design

Can I use Bootstrap to make my app responsive for this project?

#### Answer

No, Bootstrap and other CSS frameworks should not be used, all responsiveness should be done with CSS.
***

### Q4. Accessibility
How can I be sure that my work on accessibility meets the specification? 

#### Answer

You can use chrome developer tool:
1. The Accessibility pane is where you can view the accessibility tree, ARIA attributes, and computed accessibility properties of DOM nodes.

    * Go to Accessibility pane
    * Click the **Elements** tab
    * In the **DOM Tree**, select the element which you want to inspect.
    * Click the **Accessibility tab**
    * Check this full [documentation](https://developers.google.com/web/tools/chrome-devtools/accessibility/reference) for more explantation

***


### Q5. Service worker
My service worker doesn't work properly

#### Answer

Make sure to follow and check these steps:

1. Create 2 file named e.g `sw.js` & `service-worker.js`, it's recommended to save them outside `js` folder, which means in the app root folder.
2. in `sw.js` your code could be structured like this example below:
```JS
let versionOfCache = "SET A NAME VERSION"; // Whatever you want

self.addEventListener('fetch' , event => {
    event.respondWith(
        caches.match(event.request).then(response => {
            // Check caches
            if (response) return response;
            return response || fetch(event.request).then(fetchResponse =>
                caches.open(versionOfCache).then(cache => {
                    // Put file in cache next time
                    cache.put(event.request, fetchResponse.clone());
                    return fetchResponse;
                })
            ).catch(error => {
                // Logging errors if ther's a problem
                console.log('YOUR MESSAGE ERROR HERE', error);
            });
        })
    );
});

self.addEventListener('install' , event => {
    // Install cache files
    event.waitUntil(
        caches.open(versionOfCache).then(cache=> {
            cache.addAll(
                // Add images to cache without passing promise
                []
            );
            return cache.addAll(
                // Add principal files to cache
                []
            );
        })
    );
});

self.addEventListener('activate' , event => {
    // Handle old cached version
    event.waitUntil(
        caches.keys().then(cacheStored => {
            return Promise.all(
             cacheStored.map(thisCache => {
                 if (versionOfCache !== thisCache) {
                    return caches.delete(thisCache);
                 }
             })
            )
        }

        )
    );
});
```
3. In `service-worker.js` file, register your service worker in the browser:

```JS
if (navigator.serviceWorker) {
    navigator.serviceWorker.register('sw.js').then(registration => {
        console.log('Service worker is registred with scope', registration.scope);
    }).catch(e => {
        console.log(e);
    });

}
```
**PS**: This is just a starter code

4. Make sure you add this script file befor closed body tag inside both of `index.html`, `restaurant.html`files
```HTML
        <!-- Add service worker register script -->
        <script src="sw-register.js"></script>
    </body>
</html>
```


