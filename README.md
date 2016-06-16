Binary Buffer Encoder for Javascript
==================================================

This library is a buffer-based version of [BiSON](https://github.com/BonsaiDen/BiSON.js/) by BonsaiDen.

**BinSON** provides a JSON like encoding for JavaScript objects but focuses on providing a format that is optimized for use with WebSockets and other applications where bandwidth and performance are a major concern.

All the encoding and decoding are done in a single static pre-allocated buffer. Encoding a new object will overwrite the old result. 

On a NodeJS environment, the buffer will be a `Buffer` while in a browser, it will a `Uint6Array` or a regular `Array`.

To optimize the encoding of objects following a known pattern, this library can be use in conjunction with [schema-encoder](https://github.com/rainingchain/schema-encoder/).

This library can also be used with [ws-binary](https://github.com/rainingchain/ws-binary/) to send WebSockets packages very efficiently.

## Usage

The library exports a `encode` and a `decode` method.

	BinSON = require("BinSON").BinSON; 
	//BinSON is global in a browser
	BinSON.init({});
	
    // Encoding and decoding a Object
    BinSON.decode(BinSON.encode({ key: 'value' })) // { key: 'value' }

	
## Init Options

- `bufferSize`: Size in bytes of the static buffer. Encoding an object bigger than this will fail. `default=100000`

- `startOffset`: Starts the encoding at the offset specified. Useful for prepending an id. `default=0`

- `errorHandler`: Function triggered when submitting invalid data (ex: number out of range).

Example

	BinSON.init({
		bufferSize:200000,
		startOffset:0,
		errorHandler:function(msg){
			console.log(msg);
		}
	});


## Encoding Limits

- Floats are single precision
- Integers are limited to 32 bits

Numbers that are not within the valid range will be set to the limit.

`NaN` will be converted to 0.

Functions and `undefined` will be converted to `null`.  

Note: To send numbers greater than the limit, convert them to strings.

	
## Extras
	
	BinSON.compareSize(obj);
	
	BinSON.compareSpeed(obj,iterations);
	
# License

MIT.

