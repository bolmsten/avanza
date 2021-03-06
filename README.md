# Avanza

A Node.js wrapper for the unofficial Avanza API. Please note that this is only a proof of concept, hence not meant to be used by anyone.

New features will be added as soon as the crude breaks $50 so I can pay my electric bills. Contributions are more than welcome.

## Installation

Install via [npm](https://www.npmjs.com/package/avanza)

```bash
$ npm install avanza
```

or

Install via git clone

```bash
$ git clone https://github.com/fhqvst/avanza.git
$ cd avanza
$ npm install
```

## Documentation

See the [wiki](https://github.com/fhqvst/avanza/wiki).

## Test auth file

Create a `.env` file to run the tests.

```bash
# .env
USERNAME=my_username
PASSWORD=my_password
ACCOUNT=12345       # your valid account id
ORDER=12345         # the id of an order *from today* belonging to the specified account
```

## Example

Authenticate and fetch currently held positions:
```javascript
import Avanza from 'avanza'
const avanza = new Avanza()

avanza.authenticate({
    username: 'draghimario',
    password: 'cashisking1337'
}).then(() => {

   avanza.getPositions().then(positions => {
      console.log(positions);
   });

})
```

Subscribe to real-time quotes for a given instrument
```javascript
import Avanza from 'avanza'
const avanza = new Avanza()

// NOTE: use .once and not .on
avanza.socket.once('connect', () => {
  avanza.socket.subscribe('5479', ['quotes']); // Telia
});

avanza.socket.on('quotes', data => {
  console.info('Received quote: ', data);
});

avanza.authenticate({
    username: 'draghimario',
    password: 'cashisking1337'
}).then(() => {

  avanza.socket.initialize();

})
```

## Using CommonJS

The easy workaround to get going if you're using CommonJS is to call `default` when importing. See example below.
```javascript
const Avanza = require('avanza')
const avanza = new Avanza.default() // This does the trick
```
The rest should work out just the same as it does for ES6 imports.
Still having trouble? Create an [issue](https://github.com/fhqvst/avanza/issues)!

## Authentication

Logging in returns three important credentials: An authentication session, a subscription id and a security token. The first two are found in the body of the response, and the security token is found in a header: `X-SecurityToken`.
The authentication session and security token are used in every request, but the subscription id is only needed when subscribing to sockets.

The `Content-Length` header is only required when doing the authentication.

## Tests

Run all tests

```bash
$ npm test
```
NOTE: No tests will run without an `.env` file and, as surprising as it may seem, the values in it must be valid.

## LICENSE

MIT license. See the LICENSE file for details.

## RESPONSIBILITIES

The author of this software is not responsible for any indirect damages (foreseeable or unforeseeable), such as, if necessary, loss or alteration of or fraudulent access to data, accidental transmission of viruses or of any other harmful element, loss of profits or opportunities, the cost of replacement goods and services or the attitude and behavior of a third party.
