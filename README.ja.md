# ed25519-to-x25519.wasm [
![Travis CI](https://img.shields.io/travis/nazar-pc/ed25519-to-x25519.wasm/master.svg?label=Travis%20CI)
](https://travis-ci.org/nazar-pc/ed25519-to-x25519.wasm)

Ed25519署名鍵ペアを、Diffie-Hellman鍵交換に適したX25519/Curve25519鍵ペアに変換するためのWebAssemblyライブラリです。

このパッケージは、[libsodium](https://github.com/jedisct1/libsodium)のサブセットをWASMにコンパイルし、シンプルなJavaScriptラッパー関数を提供することで構築されています。

## 特徴

-   Ed25519の公開鍵と秘密鍵を、同等のX25519/Curve25519鍵に変換します。
-   WebAssemblyを活用した軽量かつ高速な処理。
-   依存関係なし。
-   Node.jsおよびモダンブラウザ（バンドラーやAMDローダー経由）で動作します。

## インストール方法

```bash
npm install ed25519-to-x25519.wasm
```

## 使用方法

ライブラリを使用する前に、WebAssemblyモジュールをロードしてコンパイルする必要があります。これはコールバックを受け取る `ready()` 関数によって処理されます。

Node.jsでの完全な例は以下の通りです：

```javascript
const ed25519_to_x25519 = require('ed25519-to-x25519.wasm');

// Ed25519鍵（Uint8Arrayとして）
const ed25519_public_key = new Uint8Array(
  Buffer.from('8fbe438aab6c40dc2ebc839ba27530ca1bf23d4efd36958a3365406efe52ccd1', 'hex')
);
const ed25519_private_key = new Uint8Array(
  Buffer.from('28e9e1d48cb0e52e437080e4a180058d7a42a07abcd05ea2ec4e6122cded8f6a0d2a6b9fd1878fd76ab20caecab666916ac3cc772fc57f8fa6e8dc3227bb8497', 'hex')
);

ed25519_to_x25519.ready(() => {
  // 公開鍵を変換
  const x25519_public_key = ed25519_to_x25519.convert_public_key(ed25519_public_key);
  console.log('X25519 Public Key:', Buffer.from(x25519_public_key).toString('hex'));
  // Expected: 26100e941bdd2103038d8dec9a1884694736f591ee814e66ae6e2e2284757136

  // 秘密鍵を変換
  const x25519_private_key = ed25519_to_x25519.convert_private_key(ed25519_private_key);
  console.log('X25519 Private Key:', Buffer.from(x25519_private_key).toString('hex'));
  // Expected: 803fcdab44e9958d2f8e4d47b5f0d481d6ddb79dd462a18ee65cabe94a9e455c
});
```

RequireJSのようなAMDローダーを使用したブラウザでの利用例：

```javascript
requirejs(['ed25519-to-x25519.wasm'], function (ed25519_to_x25519) {
    ed25519_to_x25519.ready(function () {
        // Library is ready, perform conversions here
    });
})
```

## API

### `ed25519_to_x25519.ready(callback)`

WebAssemblyモジュールを初期化します。初期化が完了すると `callback` 関数が実行されます。すべての変換関数は、このコールバック内で呼び出す必要があります。

-   `callback: () => void`

### `ed25519_to_x25519.convert_public_key(ed25519_public_key)`

Ed25519公開鍵をX25519/Curve25519公開鍵に変換します。

-   `ed25519_public_key: Uint8Array` (32バイト)
-   **戻り値:** `Uint8Array` (32バイト)、変換に失敗した場合は `null`。

### `ed25519_to_x25519.convert_private_key(ed25519_private_key)`

Ed25519秘密鍵（シードとも呼ばれます）をX25519/Curve25519秘密鍵に変換します。

-   `ed25519_private_key: Uint8Array` (64バイト)
-   **戻り値:** `Uint8Array` (32バイト)

## コントリビューション

Issueの作成やPull Requestの送信はお気軽にどうぞ。大きな変更の場合は、提案する変更について議論するために、まずIssueを作成してください。

LiveScriptのソースコード（`.ls`ファイル）を読む際は、適切なインデントを保つため、エディタのタブ幅を4スペースに設定してください。

## ライセンス

[0BSD (Zero-Clause BSD / Free Public License 1.0.0)](https://opensource.org/licenses/0BSD)
