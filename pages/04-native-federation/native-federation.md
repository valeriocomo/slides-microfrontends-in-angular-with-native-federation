---
layout: image
image: '/images/native-federation.png'
---

<div class="slidev-layout h-full grid section">
    <div class="my-auto">
        <h1>Native Federation</h1> 
    </div>
</div>

---
layout: bullets
---

# Native Federation

<v-clicks>

* Same Module Federation API
* Decoupling from a specific bundler
* Built on web standard
 
</v-clicks>

<!-- https://blog.angular.dev/micro-frontends-with-angular-and-native-federation-7623cfc5f413 -->

---
layout: section
---

# Built on web standard

---
layout: statement
---

# An *import map* is a JSON object that allows developers to control how the browser resolves module specifiers when importing JavaScript modules 

---
layout: default
---

# Native Federation
## Import map

JS
```javascript
import { name as circleName } from "https://example.com/shapes/circle.js";
import { name as squareName, draw } from "./shapes/square.js";
```
HTML
```html
<script type="importmap">
  {
    "imports": {
      "shapes": "./shapes/square.js",
      "shapes/square": "./modules/shapes/square.js",
      "https://example.com/shapes/square.js": "./shapes/square.js",
      "https://example.com/shapes/": "/shapes/square/",
      "../shapes/square": "./shapes/square.js"
    }
  }
</script>
```

Ref: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules#importing_modules_using_import_maps

---
layout: bullets
---

# Native Federation
## Setup

<v-clicks>

* Shell Config
* Remote Config
* Federation Manifest 

</v-clicks>

---
layout: default
---

# Native Federation
## Shell config
### projects/shell/federation.config.js

```javascript
const {
  withNativeFederation,
  shareAll,
} = require('@angular-architects/native-federation/config');

module.exports = withNativeFederation({
  shared: {
    ...shareAll({
      singleton: true,
      strictVersion: true,
      requiredVersion: 'auto',
    }),
  },

  skip: [
    'rxjs/ajax',
    'rxjs/fetch',
    'rxjs/testing',
    'rxjs/webSocket',
  ],
});
```

---
layout: default
---

# Native Federation
## Remote config
### projects/mfe1/federation.config.js

---
layout: default
---

```javascript
const {
  withNativeFederation,
  shareAll,
} = require('@angular-architects/native-federation/config');

module.exports = withNativeFederation({
  name: 'mfe1',

  exposes: {
    './Component': './projects/mfe1/src/app/app.component.ts',
  },

  shared: {
    ...shareAll({
      singleton: true,
      strictVersion: true,
      requiredVersion: 'auto',
    }),
  },

  skip: [
    'rxjs/ajax',
    'rxjs/fetch',
    'rxjs/testing',
    'rxjs/webSocket',
  ],
});
```

---
layout: default
---

# Native Federation
## Shell config
### projects/shell/assets/federation.manifest.json

```json
{
  "mfe1": "http://localhost:4201/remoteEntry.json",
  "mfe2": "http://localhost:4202/remoteEntry.json"
}
```