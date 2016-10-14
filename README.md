[![Travis CI](https://travis-ci.org/philix/promisify-native.svg)](https://travis-ci.org/philix/promisify-native)

# promisify-native

Converts callback-based functions to Promise-based functions.

## Install

Install with [npm](https://npmjs.org/package/promisify-native)

```bash
npm install --save promisify-native
```

## Example

```js
"use strict";

// Declare variables
const promisify = require("promisify-native");
const fs = require("fs");

// Convert the stat function
const stat = promisify(fs.stat);

// Now usable as a promise!
stat("example.txt").then(function (stats) {
    console.log("Got stats", stats);
}).catch(function (err) {
    console.error("Yikes!", err);
});
```

## Promisify methods
```js
"use strict";

// Declare variables
const promisify = require("promisify-native");
const redis = require("redis").createClient(6379, "localhost");

// Create a promise-based version of send_command
const client = promisify(redis.send_command, redis);

// Send commands to redis and get a promise back
client("ping").then(function (pong) {
    console.log("Got", pong);
}).catch(function (err) {
    console.error("Unexpected error", err);
}).then(function () {
    redis.quit();
});
```

## Handle callback multiple arguments
```js
"use strict";

// Declare functions
function test(cb) {
    return cb(undefined, 1, 2, 3);
}

// Declare variables
const promisify = require("promisify-native");

// Create promise-based version of test
const single = promisify(test);
const multi = promisify(test, {multiArgs: true});

// Discards additional arguments
single().then(function (result) {
    console.log(result); // 1
});

// Returns all arguments as an array
multi().then(function (result) {
    console.log(result); // [1, 2, 3]
});
```

### Tests
Test with nodeunit
```bash
$ npm test
```

Published under the [MIT License](http://opensource.org/licenses/MIT).
