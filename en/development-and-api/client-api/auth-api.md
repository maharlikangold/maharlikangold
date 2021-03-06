# Waves Auth API

If you want to authorize a user in your service by means of his Waves account, here's the solution. In general, you should redirect the user to the official Waves Client \([https://client.wavesplatform.com/](https://beta.wavesplatform.com/) — to be changed later\) with certain query parameters including some arbitrary data for him to sign.

That might be needed in cases when you need to work with user personal data and to be sure that a given blockchain account belongs to that user.

## Process

1. You add the Waves Auth widget to your site.
2. A user stumbles upon your site, and wants to log in using his Waves account.
3. He clicks the widget button and gets redirected to the official Waves Client, along with some random data from the widget.
4. There, the user chooses whether to log in or to cancel that chain of actions.
5. If he proceeds, the data will be signed with the user's private key.
6. The user then gets redirected back to your site, along with the signature and user's public key.
7. You check the validity of the signature against the data provided for that user.
8. If all is correct, the user is now authenticated in your service.

If the user interrupts the process, he stays on the Waves Client page.

## Details

Due to the length limitations of the query string all parameters are expressed with one character.

[**Here**](https://demo.wavesplatform.com) you can find the demo project which shows how to use Web auth API.

### Request

Example: `https://client.wavesplatform.com#gateway/auth?r=https://example.com&n=Example&d=0123456789&i=/img/logo.png&success=/wavesAuth`.

Basic path is `https://client.wavesplatform.com#gateway/auth`. Then the query parameters go.

#### Referrer

`?r=https://example.com` — the URL of your service. It should be HTTPS-only. Required.

#### Name

`?n=Service%20Name` — the name of your service. Required.

#### Data

`?d=randomChars` — the data which is signed by the user's private key \(Required\).

#### Icon path

`?i=/path/to/the/icon.png` — a path relative to the Referrer parameter. It hosts the logo of your app. \(Optional\).

#### Success path

`?s=/path/to/an/API/method` — a path to the method which redirect the user while the signing is successful. By default the user is redirected to the referrer root \(Optional\).

#### Debug mode

`?debug=true` — a flag to display error messages \(Optional\).

### Response

Example: `https://example.com/wavesAuth?d=0123456789&s=CvWfUUEkhVtaPzCMm4sB8iEJ6XwuMdcx4bhsCJAq3e8yhP7j64UD3aLyn9fFSK454o427raRmSn6a9FkaJpvXrc&p=2M25DqL2W4rGFLCFadgATboS8EPqyWAN3DjH12AH5Kdr&a=3PCAB4sHXgvtu5NPoen6EXR5yaNbvsEA8Fj`.

#### Signature

`?s=base58EncodedSignature` — a signature of the data which is signed by the user's private key.

#### Public key

`?p=base58EncodedPublicKey` — user's public key.

#### Address

`?a=base58EncodedAddress` — user's Waves address.

### How to check signature validity

You can use the `Waves.crypto.isValidSignature()` method from [@waves/waves-api](https://www.npmjs.com/package/@waves/waves-api) npm package.

Signature is taken from the data in the following order: a `WavesWalletAuthentication` string, then a string with your host parameter value, then a string with your data parameter value.

