When running `rollup watch` from Gradle, nothing seems to happen: there's no error, but the watch process isn't started properly either.

This started happening in rollup v4.21.1.

Steps to reproduce:
1. `yarn install`
2. `yarn watch` => works ok
3. `./gradlew watch` => watch process doesn't run

For reference: when run from Gradle, the Node stdin seems to be a socket instead of a normal readable stream. `isTTY` is falsy. It looks like this influences the watch cli behaviour.

```
<ref *1> Socket {
  connecting: false,
  _hadError: false,
  _parent: null,
  _host: null,
  _closeAfterHandlingError: false,
  _readableState: ReadableState {
    state: 4144,
    highWaterMark: 16384,
    buffer: BufferList { head: null, tail: null, length: 0 },
    length: 0,
    pipes: [],
    flowing: null,
    errored: null,
    defaultEncoding: 'utf8',
    awaitDrainWriters: null,
    decoder: null,
    encoding: null,
    [Symbol(kPaused)]: null
  },
  _events: [Object: null prototype] {
    end: [Function: onReadableStreamEnd],
    pause: [Function (anonymous)]
  },
  _eventsCount: 2,
  _maxListeners: undefined,
  _writableState: WritableState {
    objectMode: false,
    highWaterMark: 16384,
    finalCalled: false,
    needDrain: false,
    ending: true,
    ended: true,
    finished: true,
    destroyed: false,
    decodeStrings: false,
    defaultEncoding: 'utf8',
    length: 0,
    writing: false,
    corked: 0,
    sync: true,
    bufferProcessing: false,
    onwrite: [Function: bound onwrite],
    writecb: null,
    writelen: 0,
    afterWriteTickInfo: null,
    buffered: [],
    bufferedIndex: 0,
    allBuffers: true,
    allNoop: true,
    pendingcb: 0,
    constructed: true,
    prefinished: false,
    errorEmitted: false,
    emitClose: false,
    autoDestroy: true,
    errored: null,
    closed: false,
    closeEmitted: false,
    writable: false,
    [Symbol(kOnFinished)]: []
  },
  allowHalfOpen: false,
  _sockname: null,
  _pendingData: null,
  _pendingEncoding: '',
  server: null,
  _server: null,
  fd: 0,
  [Symbol(async_id_symbol)]: 3,
  [Symbol(kHandle)]: Pipe { reading: false, [Symbol(owner_symbol)]: [Circular *1] },
  [Symbol(lastWriteQueueSize)]: 0,
  [Symbol(timeout)]: null,
  [Symbol(kBuffer)]: null,
  [Symbol(kBufferCb)]: null,
  [Symbol(kBufferGen)]: null,
  [Symbol(kCapture)]: false,
  [Symbol(kSetNoDelay)]: false,
  [Symbol(kSetKeepAlive)]: false,
  [Symbol(kSetKeepAliveInitialDelay)]: 0,
  [Symbol(kBytesRead)]: 0,
  [Symbol(kBytesWritten)]: 0
}
```
