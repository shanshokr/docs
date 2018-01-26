
# Requesting Credentials

  The first and most basic step you should take is to allow your user to connect their uPort to your app. The `requestCredentials` method is how you accomplish this, similar in concept to logging in , except there is no server session for you to manage. All you need to do to "connect" is to disclose the requested credentials you have in your uPort identity.

## Calling the request method

**By default** the `uport-connect` library will fire a QR image inside of an injected global modal to help you get up and running quickly.

**This can be disabled** by intercepting the URI so you may use another library to customize the look and feel of the QR image. See [Custom QR Styling](#custom-qr-styling)

Once the user has scanned the displayed QR image, and has submitted their credentials, the promise should resolve with a Schema.org person JSON data payload. You can then handle this data however you desire in the then function.

```js
// Basic usage with modal injection
uport.requestCredentials()
     .then((userProfile) => {
       // Do something after they have disclosed credentials
})
```

The expected payload should look like:

```js
{
  "@context":"http://schema.org",
  "@type":"Person",
  "name":"Agent Smith",
  "address":"23fga3r2hh87ddhq98dhas8dz101j9f449w0",
  "avatar": {
    "uri": "https://ipfs.infura.io/ipfs/QmaqGAeHmwAi44T6ZrSuu3yxwiyHPxoE1rHGmKxeCuZbS7DBX"
  },
  "country": "US"
  "network":"rinkeby",
  "publicEncKey": "dgH1devHn5MhAcph+np8MI4ZLB2kJWqRc4NTwtAj6Fs="
  "publicKey":"0x04016751595cf2f1429367d6c83a826526g613b4f7574af55ded0364f0fb34600bceba9211e5864ae616d7e83b5e3c79f1c913b40c8d38c64952fef383fd3ad637",
}
```

## Requesting specific credentials

You can request specific credentials by submitting an array of values in an array of the `requested` key of a passed object.

```js
uport.requestCredentials({
  requested: ['name', 'avatar', 'phone', 'country'],
  }).then((userProfile) => {
    // Do something after they have disclosed credentials
})
```

## Enabling Push Notifications

When a transaction is going to be signed, if the `notifications` flag is set to `true` **it will allow any future transaction signing to fire a prompt in the uPort mobile app.** For UX considerations, we encourage developers to use this, otherwise your users will have to scan a QR code per each interaction.

```js
uport.requestCredentials({
  requested: ['name', 'avatar', 'phone', 'country'],
  notifcations: true
  }).then((userProfile) => {
    // Do something after they have disclosed credentials
})
```

## Custom QR Styling (web)

We have had success with the [KJUA QR Library](https://larsjung.de/kjua/). It's also recommended you wrap the QR or create a seperate button for mobile.

```js
uport.requestCredentials({
  requested: ['name', 'avatar', 'phone', 'country'],
  notifcations: true,
  (uri) => {

    const qr = kjua({
      text: uri,
      fill: '#000000',
      size: 400,
      back: 'rgba(255,255,255,1)'
    })

    // Create wrapping link for mobile touch
    let aTag = document.createElement('a')
    aTag.href = uri

    // Nest QR in <a> and inject
    aTag.appendChild(qr)
    document.querySelector('#kqr').appendChild(aTag)
  }
  }).then((userProfile) => {
    // Do something after they have disclosed credentials
})
```

## Logging in via Mobile (sdk)

**Under construction**