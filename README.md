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
- Floats greater than 32 bits will be considered as integers.
- Integers are limited to 48 bits
- Encoded integers above 32 bits can slightly differ from the real value.

Integers that are greater than 48 bits will not be encoded correctly. Note: `Date.now()` is less than 48 bits.

`NaN` will be converted to 0.

Functions and `undefined` will be converted to `null`.  

	
## Extras
	
	BinSON.compareSize(obj);
	
	BinSON.compareSpeed(obj,iterations);
	
# Formatting Differences with BiSON

Integers marked with `6` encodes on 32 bits instead of 24.

Integers marked with `7` encodes on `48` bits instead of 32.

Integers greater than 32 bits or split into `high` and `low` values when performing the binary operations.


# License

MIT.

