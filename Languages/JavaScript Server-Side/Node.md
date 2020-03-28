# Node

[TOC]



## Node

- JavaScript runtime, outside of browser
- cross-platform, device-independent
- asynchronous, event-driven
- many APIs like in browser, e.g. `console`
- → can use JS to easily build cross-platform scripts / applications, instead of bash
- → rich ecosystem of packages, open source community



## Terminology

### Modules

- `.js` file that exports a function, can be imported in another `.js`, e.g. using `require()` in Node.js
- can be package but doesn't need to

### Packages

- folder containing a `package.json` and a `.js` file that is referenced as `main` property in `package.json`, by default is called `index.js`
- packages are modules, but modules are not necessarily packages
- device-independent packages, need to be coded properly using built-in modules, e.g. `path.join()` instead of string cocatening file paths



## Events

- events work differently than in browser
- events are triggered manually, no browser window that magically triggers events through interaction
- there are no built-in events, everything is created manually
- there are no event objects, can pass arguments to EH when triggering event, event is just event name
- event targets are called event emitters, more accurate name
- there is no event flow

### Event emitters

- EM objects are instances of `EventEmitter`
- `eventEmitter.attachListener()`/`on()`: adds EH to EM for event name, returns EM so calls can be chained
- `eventEmitter.removeListener()`/`off()`: removes EH from EM for event name, returns EM so calls can be chained
- `eventEmitter.once()`: adds one-time EH to EM for event name, returns EM so calls can be chained, i.e. like `on()` for an EH which calls `off()` inside itself
- `eventEmitter.emit()`: calls EHs for event name _synchronously_, in order of registration, passing arguments to each, returns boolean if any EHs are attached

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

myEmitter.on('something', () => {
  console.log('Reacting to event...');
});

myEmitter.on('error', err => {
  console.error('Whoops, an error occurred!', err);
});

myEmitter.emit('something');
// Reacting to event...

myEmitter.emit('error', new Error('Illegal operation.'));
// Whoops, an error occurred! Error: Illegal operation.
```

### Event handlers

- EHs for same event are called _sync_ and in order of attachment, must be non-async functions ❗️
- `this` inside EH refers to EM which EH is registered on
- return value of EH is ignored
- can attach at most `10` EHs for given event by default, more will output a trace warning to `stderr`, can change using `eventEmitter.setMaxListeners()`
- if EH is attached multiple times for same event and EM, it is called multiple times, needs to be removed multiple times as well ❗️
- if no EH is attached for `'error'` event, the first argument is thrown, a stack trace is printed, and the Node.js process exits, therefore always attach an error handler ❗️
- removing EHs won't stop calls for already emitted events



## Streams

- sequence of data chunks
- operate on one chunk at a time instead of whole file
- keeps memory usage low, by default chunks are 16kb
- doesn't need to wait for complete data, can start processing immediately as chunks are received
- used to handle files, don't load complete file into memory and only then process it, even if done asynchronously is memory inefficient, e.g. large files from harddrive or from slow network, like on server to multiple clients, e.g. like a video streams from YouTube
- imagine doing all homework once on a single day or spacing it out over multiple days, the latter has less mental stress because works only on a part, also can present teacher the completed parts already before finished the rest, can quit homework early if teacher doesn't want it anymore, or can do other homework in meantime if teacher is running late on seeing it through, i.e. doing operation repeatedly on smaller chunks is better than doing it all once
- can hook up multiple streams after each other, modify data chunks
- think about it as gardenhose from tap into bucket, chain multiple gardenhoses together
- consider consuming data as stream, especially if comes through slow network, but needs to set up source to emit streams

### Types of streams

- readable: streams that can read from, e.g. `fs.createReadStream()`
- writable: streams that can write to, e.g. `fs.createWriteStream()`
- duplex: streams that are readable and writable, e.g. `net.Socket`
- transform: duplex streams that can modify data while it's read / written, e.g. `zlib.createDeflate()`

### Implementation

- streams are instances of `EventEmitter`, i.e. event target objects to which can attach event handlers
- readable stream reads chunks of data from underlying resource into internal queue, emits `data` event as soon as data is available in queue to be read from it and `end` if stream is complete ???
- writable stream writes data from internal queue to the underlying resource, emits `drain` event as soon as can continue to write, `true`/`false` events if write was un/successful ???
- handlers operate on the data chunk, e.g. for readable stream on `data` event, for writable stream on `drain` event ???
- like creating a promise, except it "resolves" over and over until all chunks are processed
- have an internal buffer to store data if it is not consumed fast enough by handler, if exceeds chunk size will stop temporarily until cache is cleared
- needs a backpressure buffer, i.e. read stream must be cached and throttled down if write stream is too slow, "overflow prevention" of writable stream

### Usage

- existing streams already implement reading from / writing to underlying resource
- just needs to add the appropriate event handlers

#### Readable stream

- paused mode: `stream.read()` must be called manually to read chunks of data, every stream starts in paused mode
- flowing mode: chunks of data are read automatically and provided via events, can turn on by adding a `'data'` event handler or calling `stream.resume()`, can switch back to paused mode by calling `stream.pause()`
- if no `'data'` event handler is attached and stream is in flowing mode, data is lost ❗️
- 






### `pipe()`

- method on readable stream
- passes reading stream to writing stream
- handles all event handler logic, e.g. putting readable stream in flow mode
- hides all the event handler logic, attaches listeners directly, does the work of errors, end of file, etc.
- implements backpressure buffering
- returns itself a readable stream such that can pipe to it again


```javascript
// stream1 is readable
// stream2 is writable

stream1.pipe(stream2);
```

- can cancel stream operation ????????


### Built-in streams

`fs.createReadStream()`: read file into stream
`fs.createWritableStream()`: write stream to file system




- Readable: a stream you can pipe from, but not pipe into (you can receive data, but not send data to it). When you push data into a readable stream, it is buffered, until a consumer starts to read the data.
- Writable: a stream you can pipe into, but not pipe from (you can send data, but not receive from it)
- Duplex: a stream you can both pipe into and pipe from, basically a combination of a Readable and Writable stream
- Transform: a Transform stream is similar to a Duplex, but the output is a transform of its input