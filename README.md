# Introduction to JavaScript

## History

![](img/eich.jpg)

- Designed and implemented as *LiveScript* over 10 days in 1995 by Brendan Eich at Netscape
- Main influences:
  - Scheme (Functions)
  - Self (Prototypes)
  - Java (Syntax)
  - Perl (Regular expressions)
- Released in 1996 as *JavaScript*&trade; for marketing purposes
- Standardized as *EcmaScript* in 1997
- Long version gap between 2005 and 2015
  - Time for browser implementers to catch up
- Yearly releases since 2015 (ES6)

## Platforms

 ```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<link     rel="stylesheet"     href="pretty.css">
<script  type="text/javascript" src="jquery-3.6.0.js"></script>
</head>
<body>

<div style="display: flex">
    <textarea id="output" style="flex: 0px" rows="50"></textarea>
</div>

</body>
<script  type="text/javascript">
// ...
$.get("README.md" /* ... */)
// ...
document.getElementById("output").value = body;
// ...
document.querySelector("#output").value = error;
// ...
</script>
</html>
```

- Browser (frontend)
  - Trinity:
    - HTML (*H*yper*T*ext *M*arkup *L*anguage)
    - CSS (*C*ascading *S*tyle *S*heets)
    - JS (*J*ava*S*cript)
  - Form validation
  - DOM manipulation (*D*ocument *O*bject *M*odel)
  - Full-blown web applications
    - Gmail
    - Google Maps
    - Google Docs
- Node.js (backend)
  - JavaScript runtime environment built on Chrome's V8 JavaScript engine
  - High-performance, asynchronous server code
- Electron = Chromium + Node.js (cross-platform desktop applications)
  - Atom
  - Discord
  - Microsoft Teams
  - Skype
  - Visual Studio Code

## Type system

- Dynamically typed
- Primitives
  1. `undefined`
     - uninitialized variables
     - function calls without return value
     - missing function arguments
     - missing object properties
     - array index out of bounds
  2. `boolean`
     - `false`
     - `true`
  3. `number` (double precision floating point)
     - `0.1 + 0.2 == 0.30000000000000004`
     - Contains all integers up to `9_007_199_254_740_992` (9 quadrillion, 9 Billiarden)
  4. `bigint` (arbitrary precision integer)
     - `123n`
     - Supports the same operators as double, except `>>>`
  5. `string` (UTF-16)
     - `"Ain't no sunshine"`
     - `'The cow says: "Moo"'`
     - `` `Frantic fans: "It's great!"` ``
  6. `null`
- `Object`
  - `Function`
  - `Array`
  - ...

### The `typeof` operator

```js
typeof undefined === "undefined"
typeof false     === "boolean"
typeof 3.14      === "number"
typeof 123n      === "bigint"
typeof "hello"   === "string"
typeof "hi"[0]   === "string"
typeof null      === "object"

typeof {}        === "object"
typeof log       === "function"
typeof []        === "object"
[].constructor   === Array
```

## Variables

- `var` variables have function scope:

```js
function f() {
    // scope of x starts here
    // ...
    {
        // ...
        var x = 42;
        // ...
    }
    // ...
}   // scope of x ends here
```

- `let` variables have block scope:

```js
function f() {
    // ...
    {
        // ...
        let x = 42;
        // scope of x starts here
        // ...
    }   // scope of x ends here
    // ...
}
```

- `const` requires initialization and forbids assignment:

```js
function f() {
    const uninitialized; // error: const requires initialization

    const x = 42;
    x = 97;              // error: const forbids assignment

    const account = new Account();
    account.deposit(42);     //    ok: method call is not assignment
    account = new Account(); // error: const forbids assignment
}
```

## Equality

https://stackoverflow.com/a/23465314

- Type-safe equality via `===` and `!==` (compares type and value)

![](img/eqeqeq.png)

- Type-unsafe equality via `==` and `!=` (confusing type coercions)

![](img/eqeq.png)

- There is no universal `equals` method for object equality

## Truthy vs. falsy values

```js
function probe(x) {
    if (x) {
        console.log("truthy");
    } else {
        console.log("falsy");
    }
}

probe(undefined); // falsy
probe(false);     // falsy
probe(+0.0);      // falsy
probe(-0.0);      // falsy
probe(NaN);       // falsy
probe(0n);        // falsy
probe("");        // falsy
probe(null);      // falsy
```

- All other values are considered truthy

## Control flow

### Conditionals

```js
let coin;
if (Math.random() < 0.5) {
    coin = "heads";
} else {
    coin = "tails";
}

const coin = Math.random() < 0.5 ? "heads" : "tails";
```

### switch/case

```js
switch (expression) {
    case 42: // ...
    break;

    case "hello": // ...
    break;

    case true: // ...
    break;

    case f(): // ...
    break;

    default: // ...
}
```

### Loops

```js
for (let i = 0; i < s.length; ++i) {
    console.log(s[i]);
}

let x = 1;
while (x + 1 > x) {
    x *= 2;
}
console.log(x + " is the smallest integer without odd successor");

let password;
do {
    password = readPassword();
} while (password !== "Simsalabim");
```

### Exceptions

```js
try {
    // ...
} catch (ex) {
    // ...
} finally {
    // ...
}
```

- A single `catch` block catches all exceptions
- At least one of `catch` and `finally` must be present
- All JavaScript values, including primitives, can be thrown

```js
throw null;
throw undefined;
throw true;
throw 42.0;
throw "string literal";

throw new Error("error object");

throw { message: "object literal" };

throw function () { };
```

## Functions

```js
// ReferenceError: Cannot access 'min' before initialization
console.log(min(3, 4));

// ReferenceError: Cannot access 'max' before initialization
console.log(max(3, 4));

// TypeError: arithmeticMean is not a function
console.log(arithmeticMean(3, 4));

// 5
console.log(geometricMean(3, 4));


let min = function (x, y) {
    return x < y ? x : y;
};

const max = function (x, y) {
    return x > y ? x : y;
};

var arithmeticMean = function (x, y) {
    return (x + y) / 2;
};

function geometricMean(x, y) {
    return Math.sqrt(x * y);
}
```

- Missing arguments are initialized to `undefined`
- Extra arguments are ignored

### Generator functions

```js
function* fibonacciSequence() {
    let a = 0n;
    yield a;

    let b = BigInt(1);
    yield b;

    while (true) {
        const c = a + b;
        yield c;

        a = b;
        b = c;
    }
}

for (const fib of fibonacciSequence()) {
    if (fib >= 1000) break;
    console.log(fib);
    // 0n
    // 1n
    // 1n
    // 2n
    // 3n
    // 5n
    // 8n
    // 13n
    // 21n
    // 34n
    // 55n
    // 89n
    // 144n
    // 233n
    // 377n
    // 610n
    // 987n
}
```

- `function*`s are stackless coroutines
  - Implemented via state machines
  - Similar to C# iterators

### Higher-order functions

```js
function* fixCos() {
    let x = 0;
    do {
        yield x;
    } while (x !== (x = Math.cos(x)));
}

for (const x of fixCos()) {
    console.log(x);
    // 0
    // 1
    // 0.5403023058681397
    // 0.8575532158463934
    // 0.6542897904977791
    // 0.7934803587425656
    // 0.7013687736227565
    // ...
    // 0.7390851332151608
    // 0.7390851332151606
    // 0.7390851332151607
}


function* fixSqrt() {
    let x = 0.5;
    do {
        yield x;
    } while (x !== (x = Math.sqrt(x)));
}

for (const x of fixSqrt()) {
    console.log(x);
    // 0.5
    // 0.7071067811865476
    // 0.8408964152537146
    // 0.9170040432046712
    // 0.9576032806985736
    // 0.9785720620877001
    // 0.9892280131939755
    // ...
    // 0.9999999999999997
    // 0.9999999999999998
    // 0.9999999999999999                
}


function* fix(f, x) {
    do {
        yield x;
    } while (x !== (x = f(x)));
}

for (const x of fix(Math.cos, 0)) {
    console.log(x);
}

for (const x of fix(Math.sqrt, 0.5)) {
    console.log(x);
}
```

- Functions are first class, hence functions can be:
  - passed as arguments
  - returned as results
  - stored in properties

### Closures

```js
function hello(user) {
    return `Hello, ${user}!`;
};

hello("Fred")   // "Hello, Fred!"


function goodbye(user) {
    return `Goodbye, ${user}!`;
};

goodbye("Fred") // "Goodbye, Fred!"



function greet(prompt) {
    return function (user) {
        return `${prompt}, ${user}!`;
    };
}


hello = greet("Hello");

hello("Fred")            // "Hello, Fred!"

greet("Hello")("Fred")   // "Hello, Fred!"


goodbye = greet("Goodbye");

goodbye("Fred")          // "Goodbye, Fred!"

greet("Goodbye")("Fred") // "Goodbye, Fred!"
```

- Functions have access to their surrounding context
  - Even after the enclosing function has returned!

### Variadic functions

```js
function logs(...xs) {
    for (const x of xs) {
        console.log(x);
    }
}

logs(42, "hello world", true);
```

### Arrow functions

```js
adults = people.filter(function (person) { return person.age >= 18; });

adults = people.filter(person => person.age >= 18);
```

- Great fit as arguments to higher-order functions
- JavaScript hipsters prefer arrow functions everywhere

## Objects

- A JavaScript object is essentially a `java.util.LinkedHashMap<String, Object>`
- Quotation marks around keys in object literals are optional

```js
// object literal
const inventor = { "surname": "Eich", forename: "Brendan" };

// lookup properties
inventor.forename   // 'Brendan'
inventor["surname"] // 'Eich'

// add properties
inventor["age"]   = 60;           // { surname: 'Eich', forename: 'Brendan', age: 60 }
inventor.language = "JavaScript"; // { surname: 'Eich', forename: 'Brendan', age: 60, language: 'JavaScript' }

// remove property
delete inventor.forename;     // { surname: 'Eich', age: 60, language: 'JavaScript' }
```

- Properties are accessed:
  - unquoted after dot, or
  - quoted inside brackets
- Objects are class-free
  - Properties can be added and removed at will

### Factory functions

```js
function createAccount(initialBalance, accountId) {
    return {
        balance: initialBalance,
        id: accountId,

        deposit: function (amount) {
            this.balance += amount;
        },

        getBalance: function () {
            return this.balance;
        },
    };
}

const account = createAccount(1000, 42);
// { balance: 1000, id: 42, deposit: [Function: deposit], getBalance: [Function: getBalance] }
account.deposit(234);
account.getBalance() // 1234

createAccount(1234, 42).getBalance === account.getBalance // false
```

### Object inheritance

```js
const accountMethods = {
    deposit: function (amount) {
        this.balance += amount;
    },

    getBalance: function () {
        return this.balance;
    },
};

function createAccount(initialBalance, accountId) {
    return {
        __proto__: accountMethods,

        balance: initialBalance,
        id: accountId,
    };
}

const account = createAccount(1000, 42);
// { balance: 1000, id: 42 }
account.__proto__
// { deposit: [Function: deposit], getBalance: [Function: getBalance] }
account.deposit(234);
account.getBalance() // 1234

createAccount(1234, 42).getBalance === account.getBalance // true
```

- Property lookup `o.x` starts at `o` and climbs the inheritance chain:
  - `o.x`
  - `o.__proto__.x`
  - `o.__proto__.__proto__.x`
  - `o.__proto__.__proto__.__proto__.x`
  - etc.

### Constructor functions

```js
function Account(initialBalance, accountId) {
    this.balance = initialBalance;
    this.id = accountId;
}

// Account.prototype = { constructor: Account };

Account.prototype.deposit = function (amount) {
    this.balance += amount;
};

Account.prototype.getBalance = function () {
    return this.balance;
};

             // Account.call({ __proto__: Account.prototype }, 1000, 42)
const account = new Account(1000, 42);
// Account { balance: 1000, id: 42 }
account.__proto__
// { deposit: [Function (anonymous)], getBalance: [Function (anonymous)] }
account.deposit(234);
account.getBalance() // 1234

new Account(1234, 42).getBalance === account.getBalance // true
```

- By convention, functions starting with an uppercase letter are *constructor functions*
  - Must be invoked with `new` to create `{ __proto__: F.prototype }`
  - Otherwise, `this` is `undefined`
- *Every* function has an associated `prototype` property
  - But it's only useful for constructor functions

| Function call syntax      | `this`      |
| ------------------------- | :---------: |
| `f(x, y, z)`              | `undefined` |
| `obj.f(x, y, z)`          | `obj`       |
| `new F(x, y, z)`          | `{ __proto__: F.prototype }` |
| `f.apply(obj, [x, y, z])` | `obj`       |
| `f.call(obj, x, y, z)`    | `obj`       |
| `f.bind(obj)(x, y, z)`    | `obj`       |


&nbsp;

![](img/proto.svg)

### The `class` keyword

```js
class Account {
    constructor(initialBalance, accountId) {
        this.balance = initialBalance;
        this.id = accountId;
    }

    deposit(amount) {
        this.balance += amount;
    }

    getBalance() {
        return this.balance;
    }
}

const account = new Account(1000, 42);
// Account { balance: 1000, id: 42 }
account.__proto__
// {}
account.__proto__ === Account.prototype // true
account.__proto__.deposit               // [Function: deposit]
account.__proto__.getBalance            // [Function: getBalance]
```

- JavaScript has no runtime notion of classes
- The `class` keyword merely coats syntactic sugar over the prototype system
- Arguably the most controversial feature in ES6
  - Java programmers love it
  - JavaScript programmers hate it

> **Douglas Crockford:** `class` was the most requested new feature in JavaScript.
> All of the requests came from Java programmers who have to program in JavaScript and don't want to have to learn how to do that.
> They wanted something that looks like Java so that they could be more comfortable.
>
> [Nordic.js 2014 • Douglas Crockford - The Better Parts](https://www.youtube.com/watch?v=PSGEjv3Tqo0&t=315)

## Arrays

```js
const primes = [2, 3, 5, 7];
typeof primes                      // 'object'
Object.getOwnPropertyNames(primes) // [ '0', '1', '2', '3', 'length' ]

primes.length // 4
primes[0]     // 2
primes[4]     // undefined

primes[4] = 11;      // [ 2, 3, 5, 7, 11 ]
primes.push(13);     // [ 2, 3, 5, 7, 11, 13 ]
primes.push(17, 19); // [ 2, 3, 5, 7, 11, 13, 17, 19 ]
primes[24] = 97;     // [ 2, 3, 5, 7, 11, 13, 17, 19, <16 empty items>, 97 ]
primes.length = 5;   // [ 2, 3, 5, 7, 11 ]
primes.pop();        // [ 2, 3, 5, 7 ]
```

- JavaScript arrays are JavaScript objects with a special `length` property
  - The keys are stringified numbers
- The (mutable!) `length` property is the largest index +1
- There are no "array index out of bounds" errors:
  - Reading from such an index gives `undefined`
  - Writing to such an index grows the array

### Iteration

```js
for (let i = 0; i < primes.length; ++i) {
    console.log(primes[i]);
}

primes.forEach(function (element, index, array) {
    console.log(element);
});

for (const p of primes) {
    console.log(p);
}
```

### Sorting

```js
// sort objects by their toString representation
primes.sort();
// [ 11, 13, 17, 19, 2, 3, 5, 7 ]

// sort numbers by their numeric value
primes.sort((a, b) => a - b);
// [ 2, 3, 5, 7, 11, 13, 17, 19 ]
```

## Asynchronous programming

- Problem: CPU is much faster than I/O
  - Especially HTTP roundtrips
  - Awaiting I/O result should not freeze UI

### Synchronous File I/O

```java
// T = 0
String string = Files.readString(Path.of("C:/Users/fred/Downloads/README.md"));
// T = 5ms
System.out.println(string);
```

### Synchronous HTTP roundtrip

```java
// T = 0
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder().uri(uri).build();
// T = 479ms
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
// T = 977ms
System.out.println(response.body());
```

- Synchronous I/O blocks current thread until result is available
- Swing: Blocking the [Event Dispatch Thread](https://docs.oracle.com/javase/tutorial/uiswing/concurrency/dispatch.html) freezes UI

> It's useful to think of the code running on the event dispatch thread as a series of **short tasks**.
> Most tasks are invocations of event-handling methods, such as `ActionListener.actionPerformed`.
> Tasks on the event dispatch thread **must finish quickly**;
> if they don't, unhandled events back up and the **user interface becomes unresponsive**.

- Spring: Every request is handled concurrently in a separate, pooled thread

### Asynchronous HTTP roundtrip

```java
// T = 0
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder().uri(uri).build();
// T = 484ms
CompletableFuture<HttpResponse<String>> future = client.sendAsync(request, HttpResponse.BodyHandlers.ofString());
// T = 526ms   Thread[main,5,main]
future.thenAccept((HttpResponse<String> response) -> {
    // T = 980ms   Thread[ForkJoinPool.commonPool-worker-3,5,main]
    System.out.println(response.body());
});
// T = 528ms   Thread[main,5,main]
continueWithOtherWork();
```

- Asynchronous I/O keeps current thread busy
- Response will be handled concurrently by another thread

### Event Loop

- The JavaScript programming language has no counterpart to `java.lang.Thread`
  - All JavaScript functions use the same call stack
  - No 2 JavaScript functions execute concurrently (barring Web Workers)
- Instead, an infinite *Event Loop* executes JavaScript callbacks sequentially from a queue
  - Every JavaScript callback runs to completion
  - No interruption/interleaving/scheduling magic
- The queue is populated concurrently by the underlying JavaScript runtime environment
  - JavaScript runtime environments use C++ threads internally
  - But those C++ threads do not manifest in the JavaScript programming language

![](img/queue.svg)

- Significantly reduced language complexity
  - Absence of "memory model" (regulates inter-thread memory visibility)
  - Race conditions, lost updates, deadlocks etc. are non-issues
- DOM is rendered between 2 callbacks, *never* during current callback!
  - Long-running functions cause intermittent freezes
  - Infinite loops freeze everything forever:

```js
setTimeout(function () {
    // This callback never leaves the queue...
    console.log("A miracle!");
}, 0);
while (true) {
    // ...because the call stack never drains
}
```

### Oversimplified implementation

```java
var queue = new LinkedBlockingQueue<Runnable>();

void eventLoop() {
    while (true) {
        Runnable callback = queue.take();    // blocks if empty
        callback.run();
        // render DOM between 2 callbacks
        DocumentObjectModel.render();
    }
}

var httpClient = HttpClient.newHttpClient();

void get(URI uri, Consumer<HttpResponse<String>> callback) {
    var request = HttpRequest.newBuilder().uri(uri).build();
    var future = httpClient.sendAsync(request, HttpResponse.BodyHandlers.ofString());

    future.thenAccept(response -> {
        queue.add(() -> callback.accept(response)); // unblocks
    });
}
```

### Asynchronous I/O in Node.js

```js
const http = require("http");
const fs   = require("fs");

const server = http.createServer(requestListener);
server.listen(8080);

function requestListener(request, response) {
    fs.readFile(__dirname + request.url, function (error, data) {
        if (error) {
            console.log("error!");
            response.writeHead(404);
            response.end(JSON.stringify(error));
        } else {
            console.log("success!");
            response.writeHead(200);
            response.end(data);
        }
    });
    console.log("file content requested...");
}
```

### Asynchronous Fetch (ES6 Promises)

```js
fetch("README.md")
.then(function (response) {
    console.log("decoding response body");
    return response.text();
})
.then(function (body) {
    console.log("success!");
    document.getElementById("output").value = body;
})
.catch(function (error) {
    console.log("error!");
    document.querySelector("#output").value = error;
});
console.log("HTTP request sent...");
```

### Asynchronous Fetch (ES8 async/await)

```js
(async function () {
    try {
        let response = await fetch("README.md");
        console.log("decoding response body");
        let body = await response.text();
        console.log("success!");
        document.getElementById("output").value = body;
    } catch (error) {
        console.log("error!");
        document.querySelector("#output").value = error;
    }
})();
```

- `async`/`await` is "just" syntactic sugar for Promise-based code
- In particular, `await` does *not* block/wait! (Misnomer?)
- Where does `console.log("HTTP request sent...");` belong now?
