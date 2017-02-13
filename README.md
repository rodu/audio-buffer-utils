# audio-buffer-utils [![Build Status](https://travis-ci.org/audiojs/audio-buffer-utils.svg?branch=master)](https://travis-ci.org/audiojs/audio-buffer-utils) [![stable](http://badges.github.io/stability-badges/dist/stable.svg)](http://github.com/badges/stability-badges)

Utility functions for [_AudioBuffers_](https://github.com/audiojs/audio-buffer) in web-audio and node.

## Usage

[![npm install audio-buffer-utils](https://nodei.co/npm/audio-buffer-utils.png?mini=true)](https://npmjs.org/package/audio-buffer-utils/)

### `const utils = require('audio-buffer-utils')`
Get utils toolset.
Data layout convention is horizontal:

<table>
  <tr>
    <th>Channel #</th>
    <th colspan="4">Sample #</th>
  </tr>
  <tr>
    <td>0</td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>...</td>
    <td>`.length`</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>...</td>
    <td>`.length`</td>
  </tr>
  <tr>
    <td colspan="5">...</td>
  </tr>
  <tr>
    <td>`.numberOfChannels`</td>
    <td>0</td>
    <td>1</td>
    <td>2</td>
    <td>...</td>
    <td>`.length`</td>
  </tr>
</table>


In that, arguments convention is: first goes sample index (x-coordinate) and then number of a channel (y-coordinate).

### `utils.create(data|length, channels = 2, sampleRate?)`
Create a new buffer from any argument.
Data can be a length, an array with channels' data, an other buffer or plain array.

```js
//mono 100-samples buffer
let a = utils.create(1, 100)

//stereo buffer with channels data
let b = utils.create([Array(100).fill(0.5), Array(100).fill(0.4)])

//minimal length buffer (1 sample)
let c = utils.create()
```

### `utils.shallow(buffer)`
Create a new buffer with the same characteristics as `buffer`, contents are undefined.

### `utils.clone(buffer)`
Create a new buffer with the same characteristics as `buffer`, fill it with a copy of `buffer`'s data, and return it.

### `utils.copy(fromBuffer, result, offset?)`
Copy the data from one buffer to another, with optional offset.

### `utils.reverse(buffer, result?)`
Reverse `buffer`. Place data to `result` buffer, if any, otherwise modify `buffer` in-place.

### `utils.invert(buffer, result?)`
Invert `buffer`. Place data to `result` buffer, if any, otherwise modify `buffer` in-place.

### `utils.zero(buffer)`
Zero all of `buffer`'s channel data. `buffer` is modified in-place.

### `utils.noise(buffer)`
Fill `buffer` with random data. `buffer` is modified in-place.

### `utils.equal(bufferA, bufferB, ...)`
Test whether the content of N buffers is the same.

### `utils.fill(buffer, result?, value|(sample, idx, channel) => sample, start?, end?)`
Fill `buffer` with provided function or value.
Place data to `result` buffer, if any, otherwise modify `buffer` in-place.
Pass optional `start` and `end` indexes.

### `utils.map(buffer, (sample, idx, channel) => newSample )`
Create a new buffer by mapping the samples of the current one.

### `utils.slice(buffer, start?, end?)`
Create a new buffer by slicing the current one.

### `utils.concat(buffer1, buffer2, buffer3, ...)`
Create a new buffer by concatting passed buffers.
Channels are extended to the buffer with maximum number.
Sample rate is changed to the maximum within the buffers.

### `utils.resize(buffer, length)`
Return new buffer based on the passed one, with shortened/extended length.
Initial data is whether sliced or filled with zeros.
Useful to change duration: `util.resize(buffer, duration * buffer.sampleRate)`

### `utils.pad(buffer, length, value?), utils.pad(length, buffer, value?)`
Right/left-pad buffer to the length, filling with value.

```js
let buf = util.create(1, 3)
util.fill(buf, .2)

util.pad(buf, 5) // [.2,.2,.2, 0,0]
util.pad(5, buf) // [0,0, .2,.2,.2]
util.pad(buf, 5, .1) // [.2,.2,.2, .1,.1]
util.pad(5, buf, .1) // [.1,.1, .2,.2,.2]
```

### `utils.shift(buffer, offset)`
Shift signal in the time domain by `offset` samples, filling with zeros.
Modify `buffer` in-place.

### `utils.rotate(buffer, offset)`
Shift signal in the time domain by `offset` samples, in circular fashion.
Modify `buffer` in-place.

### `utils.reduce(buffer, (prev, curr, idx, channel, channelData) => sample, startValue?)`
Fold buffer into a single value. Useful to generate metrics, like loudness, average, etc.

### `utils.normalize(buffer, result?, start?, end?)`
Normalize buffer by the max value, limit to the -1..+1 range.
Place data to `result` buffer, if any, otherwise modify `buffer` in-place.

### `utils.trim(buffer, threshold?)`
### `utils.trimStart(buffer, threshold?)`
### `utils.trimEnd(buffer, threshold?)`
Create buffer with trimmed zeros from the start and/or end, by the threshold.

### `util.mix(bufferA, bufferB, ratio|(valA, valB, idx, channel) => {}?, offset?)`
Mix second buffer into the first one. Pass optional weight value or mixing function.

### `utils.size(buffer)`
Return buffer size, in bytes. Use pretty-bytes package to format bytes to a string, if needed.

### `utils.data(buffer, data?)`
Get channels' data in array. Pass existing array to transfer the data to it.
Useful in audio-workers to transfer buffer to output.


## Related

> [audio-buffer](https://github.com/audio-lab/buffer) — audio data container, both for node/browser.<br/>
> [pcm-util](https://github.com/audio-lab/pcm-util) — utils for low-level pcm buffers, like audio formats etc.<br/>
> [scijs](https://github.com/scijs) — DSP utils, like fft, resample, scale etc.
