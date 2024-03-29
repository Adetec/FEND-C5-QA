
- [Memory game project](#memory-game-project)
  - [Q1. Performance (Adel)](#q1-performance-adel)
    - [Answer](#answer)
- [Arcade game project](#arcade-game-project)
  - [Q1. Changing character (Micheal)](#q1-changing-character-micheal)
    - [Answer](#answer-1)
- [Restaurant reviews project](#restaurant-reviews-project)
  - [Q1. My map doesn't show up (Adel)](#q1-my-map-doesnt-show-up-adel)
    - [Answer](#answer-2)
  - [Q2. Problem with python server (Adel)](#q2-problem-with-python-server-adel)
    - [Answer](#answer-3)
  - [Q3. Responsive design (Micheal)](#q3-responsive-design-micheal)
    - [Answer](#answer-4)
  - [Q4. Accessibility (Adel)](#q4-accessibility-adel)
    - [Answer](#answer-5)
  - [Q5. Service worker (Adel)](#q5-service-worker-adel)
    - [Answer](#answer-6)
  - [Q6. Accessibility (Contrast ratio) (Adel)](#q6-accessibility-contrast-ratio-adel)
    - [Answer](#answer-7)
  - [Q7. Accessibility (Focus) (Micheal)](#q7-accessibility-focus-micheal)
    - [Answer](#answer-8)
- [General](#general)
  - [Q1. JS & DOM (Micheal)](#q1-js--dom-micheal)
    - [Answer](#answer-9)
  - [Q2. Forgetfulness (Micheal)](#q2-forgetfulness-micheal)
    - [Answer](#answer-10)

## Memory game project

### Q1. Performance (Adel)
My game is very slow on loading, how can I improve it's performance

#### Answer
You may have external libraries loading synchronously, so try to use `prefetch` to avoid `CSS` render blocking, and `async` with Javascript files to enhance performance and load the game faster.

```HTML
<link rel="stylesheet prefetch" href="..">
<script async src="..">
```
***


## Arcade game project

### Q1. Changing character (Micheal)
I've changed the player character image to princess girl, so I replaced `char-boy.png` to `char-princess-girl.png` in the player sprite

```JS
this.sprite = 'images/char-princess-girl.png';
```
It doesn't work, and get this error

![](img/char-error.png?raw=true)

But if I leave the default boy character, the image is rendered correctly. I wonder where is the problem!

#### Answer
The game engine reloads only resources defined in the array passed into `Rescources.load()` method, take a look at `engine.js` and add your image character there:

```JS
Resources.load([
  'images/stone-block.png',
  'images/water-block.png',
  'images/grass-block.png',
  'images/enemy-bug.png',
  'images/char-boy.png',
  'images/char-princess-girl.png', // here
]);
```
Relaod the game, it should be rendred now
***


## Restaurant reviews project

### Q1. My map doesn't show up (Adel)
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

### Q2. Problem with python server (Adel)

When I run the following command line on my terminal to start the server

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

probably you have a server already running on your machine with the same port 8000, perhaps there is a process running in background and using that port, so you need to change your app port to another number:

On `db_helper.js` file of your project, change the port value

* e.g: ```const port= 8060```
* Try to run the server with updated port
```
python3 -m http.server 8060
```

If you face problems with the previous method, you can try this link:
https://www.npmjs.com/package/http-server


* Don't forget to update your `README.md` file
***



### Q3. Responsive design (Micheal)

Can I use Bootstrap to make my app responsive for this project?

#### Answer

No, Bootstrap and other CSS frameworks should not be used, all responsiveness should be done with **PURE CSS**.
***

### Q4. Accessibility (Adel)
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


### Q5. Service worker (Adel)
My service worker doesn't work properly

#### Answer

Make sure to follow and check these steps:

1. Create 2 files named e.g `sw.js` & `service-worker.js`, it's recommended to save them outside `js` folder, which means in the app root folder.
2. in `sw.js` your code could be structured like this example below:
3. 
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

4. Make sure you add this script file before closed body tag inside both of `index.html`, `restaurant.html`files
```HTML
        <!-- Add service worker register script -->
        <script src="sw-register.js"></script>
    </body>
</html>
```
***


### Q6. Accessibility (Contrast ratio) (Adel)

In web acessibility how to choose sufficient contrast ratio?

#### Answer

Make sure that the contrast ratio between text color and background color is at least 4.5, be aware also that for some people, especially people with dyslexia, a very high contrast color scheme can make reading more difficult. It’s a good idea to choose an off-white background color rather than a white background to aid on-screen reading.

To inspect the contrast ratio of an element, let's open chrome developer tool

![](img/contrast-ratio1.png?raw=true)

Let's make the background color more darker

![](img/contrast-ratio2.png?raw=true)

I recommend you using this amazing [Color Contrast Analyser (CCA)](https://developer.paciellogroup.com/resources/contrastanalyser/) tool, it helps you determine the legibility of text and the contrast of visual elements.

***


### Q7. Accessibility (Focus) (Micheal)

In lesson 2 focus - 5 deciding whats in focus: says that is counterproductive to add tabindex to elements like headers. how we focus to the important heading?

#### Answer

Important headings we focus to are those who user can interact with. For instance, **Heading links** that redirect user to another page or other section in the same page.
***


## General

### Q1. JS & DOM (Micheal)

انا عندى مشكلة فى التطبيق فى المشاريع ببقي فاهمة الاساسيات كويس جدا بس لما ببدأ أطبق على مشروع او بروجكت حقيقى بحس انى تايهة ومش عارفة ابدأ منين وده بيسبب لى احباط شديد جدا وخصوصا التطبيق على
JavaScript&DOM

#### Answer
يمكنك التواصل مع الـ `student advocates` أو زملائك إن احتجت لأي مساعدة سواء لها علاقة بالدروس أو المشاريع
لاتتردي في فعل ذلك، ففي الأخير القناة أنشئت خصبصا لمساعدة بعضنا البعض

***

### Q2. Forgetfulness (Micheal)

هل الطبيعى انى أنسى الكود وأحتاج كل شوية انى أراجعه وده بيخلينى أحس بملل ولا الأفضل اطبق ولما يقف قدامى حاجة أبدأ ابحث عن طريقة تنفيذها ؟

#### Answer

من الطبيعي جدا أن الطالب أو المبرمج ينسى المصطلحات، أو ماتعلمه عن كيفية عمل كذا أو كذا، فهذا شيء عادي، المهم  فهم الأساسيات جيدا ليتمكن من استرجاع ما قد نسيه بسرعة.

عند تعلم شيء جديد، عليك بكثرة التطبيق عليه لكي يرسخ في ذهنك عبر عمل تجارب، تمارين أو تطبيقات صغير
كتابة التعليقت وإستعمال تقنية الـ git وعمل commits في كل مرحلة تطوري فيها أكوادك يجعلك تسترجعين بسرعة ماكنت تقومين به حتى بعد  مرور وقت طويل على إنشاءه

كل مبرمج خلال بناءه تطبيق أو عمل مشاريع يقوم دائما بالبحث ويستعين بالمصادر الخارجية ويسترجع من أعماله السابقة أو الدروس التي أخذها مسبقا أو الملاحظات التي دونها سالفا، يقوم حتى بالإستعانة بأصدقائه أو اللجوء إلى المنتديات المتخصصة بالبرمجة مثل [StackOverFlow](https://https://stackoverflow.com)
***