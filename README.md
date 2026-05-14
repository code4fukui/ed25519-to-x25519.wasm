# ed25519-to-x25519.wasm [
![Travis CI](https://img.shields.io/travis/nazar-pc/ed25519-to-x25519.wasm/master.svg?label=Travis%20CI)
](https://travis-ci.org/nazar-pc/ed25519-to-x25519.wasm)

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

A WebAssembly library for converting Ed25519 signing key pairs into X25519/Curve25519 key pairs suitable for Diffie-Hellman key exchange.

This package is built by compiling a subset of [libsodium](https://github.com/jedisct1/libsodium) to WASM and providing simple JavaScript wrapper functions.

## Features

-   Converts Ed25519 public and private keys to their X25519/Curve25519 equivalents.
-   Lightweight and fast, powered by WebAssembly.
-   Zero dependencies.
-   Works in Node.js and modern browsers (via bundlers or AMD loaders).

## Installation

```bash
npm install ed25519-to-x25519.wasm
```

## Usage

The WebAssembly module must be loaded and compiled before the library can be used. This is handled by the `ready()` function, which accepts a callback.

Here is a complete Node.js example:

```javascript
const ed25519_to_x25519 = require('ed25519-to-x25519.wasm');

// Ed25519 keys (as Uint8Array)
const ed25519_public_key = new Uint8Array(
  Buffer.from('8fbe438aab6c40dc2ebc839ba27530ca1bf23d4efd36958a3365406efe52ccd1', 'hex')
);
const ed25519_private_key = new Uint8Array(
  Buffer.from('28e9e1d48cb0e52e437080e4a180058d7a42a07abcd05ea2ec4e6122cded8f6a0d2a6b9fd1878fd76ab20caecab666916ac3cc772fc57f8fa6e8dc3227bb8497', 'hex')
);

ed25519_to_x25519.ready(() => {
  // Convert the public key
  const x25519_public_key = ed25519_to_x25519.convert_public_key(ed25519_public_key);
  console.log('X25519 Public Key:', Buffer.from(x25519_public_key).toString('hex'));
  // Expected: 26100e941bdd2103038d8dec9a1884694736f591ee814e66ae6e2e2284757136

  // Convert the private key
  const x25519_private_key = ed25519_to_x25519.convert_private_key(ed25519_private_key);
  console.log('X25519 Private Key:', Buffer.from(x25519_private_key).toString('hex'));
  // Expected: 803fcdab44e9958d2f8e4d47b5f0d481d6ddb79dd462a18ee65cabe94a9e455c
});
```

For browser usage with an AMD loader like RequireJS:

```javascript
requirejs(['ed25519-to-x25519.wasm'], function (ed25519_to_x25519) {
    ed25519_to_x25519.ready(function () {
        // Library is ready, perform conversions here
    });
})
```

## API

### `ed25519_to_x25519.ready(callback)`

Initializes the WebAssembly module. The `callback` function is executed once initialization is complete. All conversion functions must be called within this callback.

-   `callback: () => void`

### `ed25519_to_x25519.convert_public_key(ed25519_public_key)`

Converts an Ed25519 public key into an X25519/Curve25519 public key.

-   `ed25519_public_key: Uint8Array` (32 bytes)
-   **Returns:** `Uint8Array` (32 bytes) or `null` if conversion fails.

### `ed25519_to_x25519.convert_private_key(ed25519_private_key)`

Converts an Ed25519 private key (also known as a seed) into an X25519/Curve25519 private key.

-   `ed25519_private_key: Uint8Array` (64 bytes)
-   **Returns:** `Uint8Array` (32 bytes)

## Contribution

Feel free to create issues and send pull requests. For large changes, please open an issue first to discuss the proposed changes.

When reading the LiveScript source code (`.ls` files), please configure your editor to use 4 spaces for tabs to ensure proper indentation.

## License

[0BSD (Zero-Clause BSD / Free Public License 1.0.0)](https://opensource.org/licenses/0BSD)