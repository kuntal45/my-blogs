[comment]: # (🚀 Supercharge Your Node.js App with node-cache: A Simple and fast NodeJS internal caching)

## 📌 Introduction

Performance is key when building scalable Node.js applications. But with growing traffic and complex logic, response times can take a hit—especially when your app makes repeated database or API calls.
**This is where caching steps in**—it helps you avoid redundant processing by storing and serving frequently accessed data efficiently.
In this blog, you'll learn about **in-memory caching using `node-cache`**, **when to use it**, and **how to implement it with real code examples**. Whether you're building a blog, e-commerce site, or an API server—this guide will help you *supercharge* your app's performance in a simple and fast way.


## ⚙️ What is Caching?
**Caching** is the process of storing copies of data in a temporary storage (cache), so it can be accessed faster next time it’s requested. Instead of recalculating or re-fetching data from a database or API, your application returns it directly from cache—saving time and resources.


## 🧰 What is `node-cache`
`node-cache` is a simple caching module that has set, get and delete methods and works a little bit like memcached. Keys can have a timeout (ttl) after which they expire and are deleted from the cache. All keys are stored in a single object so the practical limit is at around 1m keys. It is best suited for small apps or single-instance services. Implementing `node-cache` is straightforward and requires minimal setup. With `node-cache`, you can easily cache API responses, prevent duplicate database calls, and store frequently accessed data.

## ⬇️ Install `node-cache`
```bash
npm install node-cache
```

## 🧪 Example Uses
### Initialize (INIT):
```js
import NodeCache from "node-cache"; // ES6 module syntax
// or
// const NodeCache = require( "node-cache" ); // CommonJS syntax
const cache = new NodeCache(); // TTL = 100 seconds
```
**Options**:
- `stdTTL`: Default time-to-live for cache entries in seconds. (default is 0, meaning no expiration).
- `checkperiod`: How often to check for expired keys (default is 600 seconds).

```js
import NodeCache from "node-cache"; // ES6 module syntax
// or
// const NodeCache = require( "node-cache" ); // CommonJS syntax
const cache = new NodeCache( { stdTTL: 100, checkperiod: 120 } );
```
Read more about the options [here](https://www.npmjs.com/package/node-cache#initialize-init)


<br>

## 🛠️ Common Methods in `node-cache`
### Store a key (SET):
**Syntax**:
```js
cache.set(key, value, ttl);
```
Sets a key value pair. It is possible to define a ttl (in seconds). Returns true on success. If the key expires based on it's ttl it will be deleted entirely from the internal data object.

**Example**:
```js
obj = { my: "Special", variable: 42 };
 
success = cache.set( "myKey", obj, 10000 );
// true
```
---
### Store multiple keys (MSET):
**Syntax**:
```js
cache.mset(Array<{key, val, ttl?}>)
```
Sets multiple key-value pairs at once. Returns true on success.

**Example**:
```js
const data = [
  { key: "key1", val: { name: "Alice", age: 30 } },
  { key: "key2", val: { name: "Bob", age: 25 }, ttl: 10000 }
];

const success = cache.mset(data);
if (success) {
  console.log("Multiple keys set successfully");
} else {
  console.log("Failed to set multiple keys");
}
```
---
### Retrieve a key (GET):
**Syntax**:
```js
cache.get(key);
```
Returns the cached value for the given key, or undefined if the key does not exist or has expired.

**Example**:
```js
const cachedData = cache.get("myKey");
if (cachedData) {
  console.log("Cache hit:", cachedData);
} else {
  console.log("Cache miss");
}
```
---
### Take a key (TAKE):
**Syntax**:
```js
cache.take(key);
```
get the cached value and remove the key from the cache.
Equivalent to calling `get(key)` + `del(key)`.
Useful for implementing single use mechanism such as OTP, where once a value is read it will become obsolete.

**Example**:
```js
myCache.set( "myKey", "myValue" )
myCache.has( "myKey" ) // returns true because the key is cached right now
value = myCache.take( "myKey" ) // value === "myValue"; this also deletes the key
myCache.has( "myKey" ) // returns false because the key has been deleted
```
---
### Get multiple keys (MGET):
**Syntax**:
```js
cache.mget( [ key1, key2, ..., keyn ] )
```
Gets multiple saved values from the cache. Returns an empty object {} if not found or expired. If the value was found it returns an object with the key value pair.

**Example**
```js
value = myCache.mget( [ "myKeyA", "myKeyB" ] );
/*
    {
        "myKeyA": { my: "Special", variable: 123 },
        "myKeyB": { the: "Glory", answer: 42 }
    }
*/
```
---
### Delete a key (DEL):
**Syntax**:
```js
cache.del(key);
```
Delete a key. Returns the number of deleted entries. A delete will never fail.

**Example**:
```js
value = cache.del( "A" );
// 1
```
---
### Delete multiple keys (MDEL):
**Syntax**:
```js
cache.del([key1, key2, ..., keyn]);
```
Delete multiple keys. Returns the number of deleted entries. A delete will never fail.

**Example**:
```js
value = myCache.del( "A" );
// 1
 
value = myCache.del( [ "B", "C" ] );
// 2
 
value = myCache.del( [ "A", "B", "C", "D" ] );
// 1 - because A, B and C not exists
```
---
### Change TTL (TTL):
**Syntax**:
```js
cache.ttl(key, ttl);
```
Redefine the ttl of a key. Returns true if the key has been found and changed. Otherwise returns false. If the ttl-argument isn't passed the default-TTL will be used.

The key will be deleted when passing in a `ttl < 0`.

**Example**:
```js
myCache = new NodeCache( { stdTTL: 100 } )
changed = myCache.ttl( "existentKey", 100 )
// true
 
changed2 = myCache.ttl( "missingKey", 100 )
// false
 
changed3 = myCache.ttl( "existentKey" )
// true
```

---
### Get TTL (getTTL):
**Syntax**:
```js
cache.getTtl(key);
```
Receive the ttl of a key. You will get:

* undefined if the key does not exist
* 0 if this key has no ttl
* a timestamp in ms representing the time at which the key will expire

**Example**:
```js
myCache = new NodeCache( { stdTTL: 100 } )
 
// Date.now() = 1456000500000
myCache.set( "ttlKey", "MyExpireData" )
myCache.set( "noTtlKey", "NonExpireData", 0 )
 
ts = myCache.getTtl( "ttlKey" )
// ts wil be approximately 1456000600000
 
ts = myCache.getTtl( "ttlKey" )
// ts wil be approximately 1456000600000
 
ts = myCache.getTtl( "noTtlKey" )
// ts = 0
 
ts = myCache.getTtl( "unknownKey" )
// ts = undefined
```

---
### List keys (KEYS)
**Syntax**:
```js
cache.keys();
```
Returns an array of all keys currently stored in the cache.

**Example**:
```js
const allKeys = cache.keys();
console.log("All cached keys:", allKeys);
```

---
### Has key (HAS)
**Syntax**:
```js
cache.has(key);
```
Checks if a key exists in the cache. Returns true if the key is found, false otherwise

**Example**:
```js
const exists = cache.has("myKey");
if (exists) {
  console.log("Key exists in cache");
} else {
  console.log("Key does not exist in cache");
}
```

---
### Statistics (STATS):
**Syntax**:
```js
cache.getStats();
```
Returns an object containing statistics about the cache, such as hit/miss ratio, number of keys, and memory usage.

**Example**:
```js
const stats = cache.getStats();
    /*
        {
            keys: 0,    // global key count
            hits: 0,    // global hit count
            misses: 0,  // global miss count
            ksize: 0,   // global key size count in approximately bytes
            vsize: 0    // global value size count in approximately bytes
        }
    */
```

---
### Flush all data (FLUSH):
**Syntax**:
```js
cache.flushAll();
```
Removes all keys from the cache. Returns true on success.

**Example**:
```js
const flushSuccess = cache.flushAll();
if (flushSuccess) {
  console.log("Cache flushed successfully");
} else {
  console.log("Failed to flush cache");
}
```

---
### Flush the stats (FLUSH STATS):
**Syntax**:
```js
cache.flushStats();
```
Resets the cache statistics (hit/miss counts, key sizes, etc.) to zero.
**Example**:
```js
const flushStatsSuccess = cache.flushStats();
if (flushStatsSuccess) {
  console.log("Cache statistics flushed successfully");
} else {
  console.log("Failed to flush cache statistics");
}
```

---
### Close the cache:
**Syntax**:
```js
cache.close();
```
Closes the cache and releases any resources it holds. This is useful when you want to clean up the cache before shutting down your application.
**Example**:
```js
cache.close();
console.log("Cache closed successfully");
```

<br>

## Events in `node-cache`
`node-cache` emits events that you can listen to for various cache operations. Here are some common events:

### Event: `set`
**Description**: Emitted when a key-value pair is successfully set in the cache.
```js
cache.on("set", (key, value) => {
  console.log(`Key "${key}" set with value:`, value);
});
```

---
### Event: `del`
**Description**: Fired when a key has been removed manually or due to expiry. You will get the key and the deleted value as callback arguments.

```js
cache.on( "del", function( key, value ){
    // ... do something ...
});
```

---
### Event: `expired`
**Description**: Emitted when a key expires and is removed from the cache. You will get the key and the deleted value as callback arguments.
```js
cache.on("expired", (key, value) => {
  console.log(`Key "${key}" expired and removed from cache. Value was:`, value);
});
```

---
### Event: `flush`
**Description**: Fired when the cache is flushed, either manually or automatically.
```js
cache.on("flush", () => {
  console.log("Cache has been flushed");
});
```

---
### Event: `flushStats`
**Description**: Fired when the cache statistics are flushed.
```js
cache.on("flushStats", () => {
  console.log("Cache statistics have been flushed");
});
```
<br>
<br>

## 🧑‍💻Basic Implementation of `node-cache` with Node.js

To get started with `node-cache`, you first need to create a blank Node.js project and install the `node-cache` package along with `express` for creating a simple server.

```bash
npm init -y
npm install node-cache express 
```

### Step 1: Create a Cache Instance
Create a file named `cache.js` to initialize the cache instance.
```js
// cache.js
import NodeCache from "node-cache";
const cache = new NodeCache({ stdTTL: 100 }); // TTL = 100 seconds
export default cache;
```
### Step 2: Set Up the Server
Create a file named `server.js` to set up a simple Express server that uses the cache
```js
// server.js
import express from "express";
import cache from "./cache.js";
const app = express();
app.get("/data", async (req, res) => {
  const cachedData = cache.get("myData");
  if (cachedData) {
    return res.json({ fromCache: true, data: cachedData });
  }

  const fetchedData = await fakeDBCall(); // Simulate a DB/API call
  cache.set("myData", fetchedData);
  res.json({ fromCache: false, data: fetchedData });
});
app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

## ✅ When to Use `node-cache`
`node-cache` is best suited for scenarios where you need fast, in-memory caching without the complexity of distributed caching solutions. Here are some common use cases:
- **API Response Caching**: Cache responses from external APIs to reduce latency and avoid unnecessary calls.
- **Database Query Caching**: Store frequently accessed database query results to speed up response times.
- **Session Management**: Cache user sessions to avoid repeated database lookups.
- **Configuration Data**: Store application configuration settings that are read frequently but change infrequently.
- **Temporary Data**: Cache data that is only needed for a short period, such as user-generated content or temporary calculations.

## ❌ When Not to Use `node-cache`
While `node-cache` is great for many scenarios, there are cases where it may not be the best fit:
- **Large Data Sets**: If you're dealing with large data sets that exceed the memory limits of your server, consider using a distributed cache like Redis or Memcached.
- **High Availability**: If your application requires high availability and fault tolerance, `node-cache` may not be suitable since it stores data in memory and does not persist across server restarts.
- **Distributed Systems**: In a distributed system with multiple instances of your application, `node-cache` won't share cached data across instances. In such cases, use a distributed caching solution like Redis or Memcached.

## Conclusion
Caching is a powerful technique to improve the performance of your Node.js applications. By using `node-cache`, you can easily implement in-memory caching to speed up data retrieval and reduce the load on your database or external APIs. Remember to use caching judiciously, considering the size of your data and the specific needs of your application. With the right caching strategy, you can significantly enhance the responsiveness and scalability of your Node.js applications. 



## 💬 Have Questions or Suggestions?

Drop a comment below or connect with me on [LinkedIn](https://www.linkedin.com/in/kuntal-maity-8aa4612a9/) or [GitHub](https://github.com/kuntal-hub). Let’s make apps faster together! 🚀