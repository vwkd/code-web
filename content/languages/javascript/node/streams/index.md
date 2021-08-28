---
title: Streams
author: vwkd
index: 4
tags:
  - languages
  - javascript
  - node
---

- sequence of data chunks of I/O, e.g. video stream from YouTube
- operate on one chunk at a time instead of whole data
- keeps memory usage low, by default chunks are 16kb
- doesn't need to wait for complete data, can start processing immediately as chunks are received
- without streams: data is fully buffered, complete data is loaded into memory, only then can process it, memory and time inefficient, doesn't matter if done a-/sync, e.g. handle large files, or data over network, serving files for multiple clients
- with streams: data is partially buffered, data is treated in separate smaller data chunks, memory and time efficient
- can think about stream as gardenhose from tap into bucket, can chain multiple gardenhoses together, can even modify content while it is passing
- the difficulty with many chunks instead of a single one comes in form of synchronisation, because needs to wait asynchronously for each chunk to be read / written before can continue, even more difficult when piping multiple streams



## Motivation

Imagine doing all homework once on a single day or spacing it out over multiple days, the latter has less mental stress because works only on a part of the homework, also can present teacher the completed parts already before finished the rest, can quit homework early if teacher doesn't want it anymore, or can do other homework in meantime if teacher is running late on seeing it through, i.e. doing an operation repeatedly on smaller chunks is better than doing a single operation on everything



## Types of streams

- readable: stream that can read from, needs to pull data out of, acts as source of data like a tap, e.g. `fs.createReadStream()`
- writable: stream that can write to, needs to push data into, acts as sink for data like a drain, e.g. `fs.createWriteStream()`
- duplex: stream that is readable and writable, e.g. `net.Socket`
- transform: duplex stream that modifies data, can write to and read modified data from, e.g. `zlib.createDeflate()`



## Idea

- each stream has an internal buffer, imagine buffer as a glass of water
- readable stream reads data from underlying resource until its buffer is full, then stops and waits until data from buffer is consumed, then continues until it reaches end of data, i.e. does need to be started but doesn't need to be closed
- writable stream writes data from underlying resource until its buffer is empty, then stops and waits until the buffer is filled again, then continues until it is closed, i.e. doesn't need to be started but needs to be closed
- for a readable stream the glass is filled automatically and you consume it when it is full, i.e. you need to wait until it's full to read from it, don't read when it's empty
- for a writable stream you fill the glass when it is empty and it is drained automatically, i.e. you need to wait until it's empty to write to it, don't write when it's full
- backpressure: if reading stream can read faster than writing can write needs to prevent "buffer overflow" of writing stream, needs to pause readable stream when writable buffer is full, resume readable stream after writable buffer has drained, otherwise buffer would consume more memory and cripple performance



## Usage

- streams are instances that inherit from `EventEmitter` class, i.e. event target objects to which can attach event handlers
- existing streams already implement reading from / writing to underlying resource, just needs to attach the appropriate event handlers and call methods
- for readable stream needs to attach EH for event when data is read into internal buffer and can be consumed, i.e. using `"readable"` event and `read()`
- for writable stream needs to attach EH for event when data is written from internal buffer and can be filled up again, i.e. using `"drained"` event and `write()`
- to pipe multiple streams together use `pipe()` instead of manual approach, much easier



## Events and methods

- `"error"` event: emitted if an error occured in the stream, always attach an EH to a stream ❗️
- `destroy()`: destroy stream, emits `"error"` event with first argument as error, doesn't wait for readable stream to be consumed or writable stream to be drained

### Readable streams

- `read()`: returns data from the internal buffer, if empty returns `null`, also starts reading next chunk into internal buffer and emitting `readable`
- `readable` event: emitted after data is read into internal buffer, i.e. ready to call `read()`, also emitted directly before `"end"` event and `read()` returns `null` in this case 
- `"end"` event: emitted when no more data can be read, but waits until remaining data in internal buffer is consumed
- overwrites the `on()` method such that it starts to read as soon as a `"readable"` (or `"data"`) EH is attached

```javascript
// readable stream

const fs = require("fs");
const file = fs.createReadStream("./testin.txt", {
    encoding: "utf8",
    highWaterMark: 64 // 64 byte chunks for better visualisation with 1 KB file
});

file.on("error", function (err) {
    console.error("Ups, an error occurred.", err);
});

file.on("readable", function () {
    // Artificial slow down, delete setTimeout wrapper call in reality
    setTimeout(() => {
        const chunk = file.read();
        if (chunk !== null) {
            console.log("Data chunk:", chunk);
        }
    }, 500);
});

file.on("end", function () {
    console.log("Finished reading all of the data.");
});
```

- classic readable streams operate in flow mode instead of paused mode, data flows automatically instead of being pulled out manually, i.e. data is provided as argument to EH for `"data"` event instead by calling `read()`, attaching an EH for `"data"` also starts flow mode, can manually toggle mode with `resume()` and `pause()`, don't use anymore, legacy ❗️

```javascript
// classic readable stream

const fs = require("fs");
const file = fs.createReadStream("./testin.txt", {
    encoding: "utf8",
    highWaterMark: 64 // 64 B chunks for better visualisation with 1 KB file
});

file.on("error", function (err) {
    console.error("Ups, an error occurred.", err);
});

file.on("data", function (data) {
    console.log("Data chunk:", data);

    // Artificial slow down, delete in reality
    file.pause();
    setTimeout(() => {
        file.resume();
    }, 500);
});

file.on("end", function () {
    console.log("Finished reading all of the data.");
});
```

### Writable streams

- `write()`: writes data into the internal buffer, if buffer is now full returns `false` else `true`, if `false` is returned should only continue writing after `"drain"` event, i.e. handle backpressure, also starts writing to underlying resource
- `end()`: closes the stream, writes data in argument as last chunk into the internal buffer, i.e. no more data can be written to stream, emits the `"close"` event
- `"close"` event: emitted when no more data to stream can be pushed
- `"drain"` event: emitted when internal buffer is again empty

```javascript
// writable stream, doesn't handle backpressure

const fs = require("fs");
const file = fs.createWriteStream("./testout.txt");

file.on("error", function (err) {
    console.error("Ups, an error occurred.", err);
});

file.on("close", function () {
    console.log("Finished writing all of the data.");
});

file.write("beep ");
file.write("boop ");
file.end("done.");
```

### Piping streams

- manually piping streams is complicated, need to synchronise consuming readable stream and filling writable stream

```javascript
// readable stream to writable stream, doesn't handle backpressure

const fs = require("fs");
const file1 = fs.createReadStream("./testin.txt", {
    encoding: "utf8",
    highWaterMark: 64 // 64 B chunks for better visualisation with 1 KB file
});
const file2 = fs.createWriteStream("./testout.txt");

file1.on("error", function (err) {
    console.error("Ups, an error occurred.", err);
});

file2.on("error", function (err) {
    console.error("Ups, an error occurred.", err);
});

file1.on("readable", function () {
    // Artificial slow down, delete setTimeout wrapper call in reality 
    setTimeout(() => {
        const chunk = file1.read();
        if (chunk !== null) {
            file2.write(chunk);
            console.log("Data chunk:", chunk);
        }
    }, 500);
});

file1.on("end", function () {
    file2.end();
    console.log("Finished reading all of the data.");
});

file2.on("close", function () {
    console.log("Finished writing all of the data.");
});
```

- even more complicated to implement backpressure handling when writable stream drains too slowly

```javascript
// readable stream to writable stream, handles backpressure

const fs = require("fs");
const file1 = fs.createReadStream("./testin.txt", {
    encoding: "utf8",
    highWaterMark: 4096 // 4 KB chunks for better visualisation with 10 MB file
});
const file2 = fs.createWriteStream("./testout.txt");

file1.on("error", function (err) {
    console.error("Ups, an error occurred.", err);
});

file2.on("error", function (err) {
    console.error("Ups, an error occurred.", err);
});

let continueRead = true;
let shouldLogRead = true;

file1.on("readable", function () {
    if (continueRead) {
        const chunk = file1.read();
        if (chunk !== null && continueRead) {
            continueRead = file2.write(chunk);
            // log only once until next buffer overflow
            if (shouldLogRead) {
                console.log("Read chunks.");
                shouldLogRead = false;
            }
        }
    } else {
        console.log("Waiting for drain...");
        shouldLogRead = true;
    }
});

file1.on("end", function () {
    file2.end();
    console.log("Finished reading all of the data.");
});

file2.on("close", function () {
    console.log("Finished writing all of the data.");
});

file2.on("drain", function () {
    continueRead = true;
    file1.emit("readable");
});
```

- `readable.pipe(writable)`: passes data from a readable stream (source) to a writable stream (destination), hides all unnecessary detail 🎉
- implements backpressure logic, i.e. stops reading from readable stream if writable stream is not yet drained, continues as soon as has drained
- automatically closes writable stream if all data is read, i.e. calls `writable.end()` after readable stream emits `"end"` event, can disable by passing option object to `pipe()`
- `readable.unpipe()`: disconnects one or all pipes from readable and writable, needs to still `end()` writable stream ❗️

```javascript
const fs = require("fs");
const file1 = fs.createReadStream("./testin.txt", {
    encoding: "utf8"
});
const file2 = fs.createWriteStream("./testout.txt");

file1.on("error", handleError);
file2.on("error", handleError);

file1.pipe(file2);
```

- `pipe()` returns the writable stream (destination), such that can pipe to it again if it is a duplex / transform stream
- however `pipe()` doesn't forward errors, not like promise chain, needs to handle manually between every pipe

```javascript
// no error handling 👎
file1.pipe(file2).pipe(file3);

// with error handling 👍
file1.on("error', handleError)
     .pipe(file2)
     .on("error", handleError)
     .pipe(file3)
     .on("error", handleError);
```



## Built-in streams

- use streams for any serious I/O, time and memory efficient, scales better
- `fs.createReadStream()`: read file into stream, internal buffer is `64 KB` instead of `16 KB` default for readable streams
- `fs.createWritableStream()`: write stream to file system
- `process.stdin`: standard system input stream
- `process.stdout`: standard system output stream
- etc.



## Implementation

- streams can be implemented by extending the corresponding base class and implementing the necessary methods
- a readable stream must call `new stream.Readable([options])` and implement a `_read()` method
  - `push()`: pushes data into internal buffer, returns `false` if buffer is now full else `true`, emits `"readable"` event
  - `_read()`: reads data from underlying resource, calls `push()` with data, continues reading until `push()` returns `false`, on error emits `"error"`
- when first attaching an EH for `"readable"` it calls `_read()` to fill up the glass, triggering the `"readable"` event and calling the EH, in the EH can consume the glass with `read()` which then calls `_read()` to fill it back up again
- a writable stream must call `new stream.Writable([options])` and implement a `_write()` method
  - `_write()`: write data to underlying resource
- when calling `write()` it calls `_write()` directly if there is no write in progress, else it buffers it until write is done and `_write()` is called until the buffer is again empty, then emits `"drain"` event, i.e. effectively the same as if `write()` always writes into buffer as described in Usage above, see [Writable stream](#)
- a transform stream must call `new stream.Transform([options])` and implement a `_transform()` method in addition to the ones for readable and writable streams
  - `_transform()`: takes data as input, transforms it, outputs new data to `push()`
- best to use NPM module that abstracts all this setup away, e.g. [through2](https://www.npmjs.com/package/through2)



## Resources

- [substack - stream-handbook](https://github.com/substack/stream-handbook)