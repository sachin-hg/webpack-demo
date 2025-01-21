
### Modules vs. Chunks

Webpack bundles modules into chunks. At runtime, it uses specific mechanisms to load modules and chunks efficiently, ensuring caching and avoiding redundant loads.

#### 1. **Static Imports**
Static imports are transformed into `__webpack_require__` calls during the bundling process.

**Example:**
```js
// Source code
import moduleA from './moduleA';

// After Webpack bundling
const moduleA = __webpack_require__('./moduleA');
```

The `__webpack_require__` function resolves the module ID, checks the module cache, and executes the module if it hasn’t been loaded yet.

---

#### 2. **Dynamic Imports**
Dynamic imports (e.g., `import('./moduleB')`) generate separate chunks and are loaded at runtime using `__webpack_require__.e`.

**Example:**
```js
// Source code
import('./moduleB').then(module => {
  module.doSomething();
});

// After Webpack bundling
__webpack_require__.e('chunk-moduleB').then(() => {
  const module = __webpack_require__('./moduleB');
  module.doSomething();
});
```

- `__webpack_require__.e('chunkId')` is responsible for loading the dynamic chunk and returning a promise.
- Once the chunk is loaded, the corresponding module is resolved using `__webpack_require__`.

---

#### 3. **Module Cache**
Webpack uses an in-memory cache to ensure that modules and chunks are only loaded once. The cache is stored in `__webpack_require__.c`.

**Example Cache Workflow:**
```js
var moduleCache = {};

function __webpack_require__(moduleId) {
  // Check if module is in cache
  if (moduleCache[moduleId]) {
    return moduleCache[moduleId].exports;
  }

  // Create a new module and store it in the cache
  var module = moduleCache[moduleId] = {
    id: moduleId,
    loaded: false,
    exports: {}
  };

  // Execute the module function
  modules[moduleId].call(module.exports, module, module.exports, __webpack_require__);

  // Mark module as loaded
  module.loaded = true;

  // Return the exports of the module
  return module.exports;
}
```

---

#### 4. **Chunk Loading (`__webpack_require__.e`)**
This function dynamically loads chunks at runtime using `<script>` tags and tracks their loading state.

**Example Chunk Loading Workflow:**
```js
var installedChunks = { 'main': 0 }; // 0 means the chunk is loaded

__webpack_require__.e = function(chunkId) {
  if (installedChunks[chunkId] === 0) {
    return Promise.resolve();
  }

  if (installedChunks[chunkId]) {
    return installedChunks[chunkId];
  }

  var promise = new Promise((resolve, reject) => {
    installedChunks[chunkId] = [resolve, reject];
  });

  installedChunks[chunkId] = promise;

  var script = document.createElement('script');
  script.src = __webpack_require__.p + chunkId + '.js';
  document.head.appendChild(script);

  return promise;
};
```

---

#### 5. **Chunk Loading Global (`webpackChunk`)**
Webpack uses a global array (by default named `webpackChunk`) to manage chunk loading. When a chunk is loaded, it is pushed into this array, and Webpack processes it immediately.

**Example Workflow:**
```js
// The global chunk loading array
var webpackChunk = window['webpackChunk'] = window['webpackChunk'] || [];

// Push the loaded chunk into the array
webpackChunk.push([chunkId, modules]);

// Webpack processes the modules in the chunk
while (webpackChunk.length) {
  var [chunkId, chunkModules] = webpackChunk.shift();

  // Add each module in the chunk to the module cache
  for (var moduleId in chunkModules) {
    modules[moduleId] = chunkModules[moduleId];
  }

  // Mark the chunk as loaded
  installedChunks[chunkId] = 0;
}
```

---

#### 6. **Public Path (`__webpack_require__.p`)**
Specifies the base path for dynamically loaded assets.

**Example:**
```js
__webpack_require__.p = '/dist/';
```

---

#### 7. **Chunk Loading Helpers (`__webpack_require__.f`)**
Webpack uses `__webpack_require__.f` as an object containing chunk loading strategies for different environments (e.g., browser, Node.js).

**Example of `__webpack_require__.f.j`:**
```js
__webpack_require__.f.j = function (chunkId, promises) {
  var installedChunkData = installedChunks[chunkId];
  if (installedChunkData !== 0) { // 0 means already loaded
    if (installedChunkData) {
      promises.push(installedChunkData[2]);
    } else {
      // Set up a new promise for this chunk
      var promise = new Promise((resolve, reject) => {
        installedChunkData = installedChunks[chunkId] = [resolve, reject];
      });
      promises.push(installedChunkData[2] = promise);

      // Create and append the script tag
      var script = document.createElement('script');
      script.src = __webpack_require__.p + chunkId + '.js';
      script.onload = () => installedChunkData[0]();
      script.onerror = () => installedChunkData[1](new Error('Chunk load failed'));
      document.head.appendChild(script);
    }
  }
};
```

---

#### 8. **Flow of Module/Chunk Loading**
- For static imports: Modules are loaded at the time of the bundle execution.
- For dynamic imports: Chunks are loaded at runtime using the dynamic import logic.

**Diagram:**
```
Static Import  → __webpack_require__  → Module Cache
Dynamic Import → __webpack_require__.e → __webpack_require__.f.j → Load Chunk (if not cached) → webpackChunk → __webpack_require__ → Module Cache
```

This process ensures that modules and chunks are efficiently loaded and reused, minimizing redundant network requests and execution overhead.
