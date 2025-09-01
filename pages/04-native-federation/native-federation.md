---
layout: image
image: '/images/native-federation.png'
preload: true
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
### @angular-architects/native-federation

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
layout: default
---

# Native Federation
## Add library

<v-clicks>

<div>
Install dependency
```bash
npm i @angular-architects/native-federation -D
```
</div>

<div>
Shell initialization
```bash
ng g @angular-architects/native-federation:init --project shell --port 4200 --type dynamic-host
```
</div>

<div>
Remote initialization
```bash
ng g @angular-architects/native-federation:init --project mfe1 --port 4201 --type remote
```
</div>

</v-clicks>
---
layout: default
---

# Native Federation
## Configuration Setup

<v-clicks>

* Host config
* Remote config
* Federation manifest 

</v-clicks>

---
layout: default
---

# Native Federation
## Host config
### projects/shell/federation.config.js

```javascript {|2|3|7-13|8-12|15-20}
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

```javascript {|7|9-11}
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
## Federation manifest
### projects/shell/assets/federation.manifest.json

```json {|2}
{
  "mfe1": "http://localhost:4201/remoteEntry.json",
  "mfe2": "http://localhost:4202/remoteEntry.json"
}
```

---
layout: default
---

# Native Federation
## App Setup

<v-clicks>

* Host init
* Remote init
* Router setup 

</v-clicks>

---
layout: default
---

# Native Federation
## Host init
### projects/shell/main.ts

```typescript {|3|5}
import { initFederation } from '@angular-architects/native-federation';

initFederation('/assets/federation.manifest.json')
  .catch((err) => console.error(err))
  .then((_) => import('./bootstrap'))
  .catch((err) => console.error(err));
```

---
layout: default
---

# Native Federation
## Remote init
### projects/mfe1/main.ts

```typescript {|3|5}
import { initFederation } from '@angular-architects/native-federation';

initFederation()
  .catch((err) => console.error(err))
  .then((_) => import('./bootstrap'))
  .catch((err) => console.error(err));
```

---
layout: default
---

# Native Federation
## Router setup
### projects/shell/app/app.routes.ts

```typescript  {|13-17|16}
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { NotFoundComponent } from './not-found/not-found.component';
import { loadRemoteModule } from '@angular-architects/native-federation';

export const APP_ROUTES: Routes = [
  {
    path: '',
    component: HomeComponent,
    pathMatch: 'full',
  },
  // Loading remote mfe1
  {
    path: 'mfe1',
    loadComponent: () =>
      loadRemoteModule('mfe1', './Component').then((m) => m.AppComponent),
  },

  {
    path: '**',
    component: NotFoundComponent,
  },
];
```