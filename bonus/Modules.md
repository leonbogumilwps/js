## ES6 Modules

- One file per module
- Explicit `export`s and `import`s
- Browsers support modules via `<script type="module" src="app.js">` from web server
- But modules are usually bundled into a single `bundle.js` file by tools like webpack

### Named exports

```js
// trig.js
export const PI = 3.141592653589793;
const RADIANS_PER_DEGREE = PI / 180; // not exported

export function radians(degrees) {
    return degrees * RADIANS_PER_DEGREE;
}

export function degrees(radians) {
    return radians / RADIANS_PER_DEGREE;
}

export function distance(x, y) {
    return Math.sqrt(square(x) + square(y));
}

// not exported
function square(x) {
    return x * x;
}
```

### Named imports

```js
// app.js
import { PI, distance as distanceFromOrigin } from './trig';

const distance = 1.5;

console.log(PI);
console.log(distanceFromOrigin(3, 4));
```

### Namespace import

```js
// app.js
import * as trig from './trig';

const distance = 1.5;

console.log(trig.PI);
console.log(trig.distance(3, 4));
```

### Default exports

```js
// round2.js
export default function (x) {
    return Math.round(x * 100) / 100;
}
```

### Default imports

```js
// app.js
import { PI } from './trig';
import round2 from './round2';

console.log(round2(PI));
```
