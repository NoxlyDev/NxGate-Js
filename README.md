# Nx Gate Wrapper

## Installation

To install NxGate Wrapper, simply run the following command:

For npm:


```bash

npm install nxgate

```

## Dependencies

- Axios
- Node.js v20.11.10 or higher

## Usage
```js
//index.js
const NxGate = require('nxgate').default;
const { ValidationType } = require('nxgate');

async function checkLicense() {
  const userId = 'Your UserID';
  const licenseKey = 'Your LicenseKey';

  const nxGate = new NxGate(userId);

  const result = await nxGate.verify(licenseKey);

  if (result === ValidationType.VALID) {
    console.log('The Key is Valid.');
  } else {
    console.log(`The Key is invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```

### With Custom Server URL
```js
//index.js
const NxGate = require('nxgate').default;
const { ValidationType } = require('nxgate');

async function checkLicense() {
  const userId = 'Your UserID';
  const licenseKey = 'Your LicenseKey';
  const serverUrl = 'https://mybackendserver.com';

  const nxGate = new NxGate(userId).setValidationServer(serverUrl);

  const result = await nxGate.verify(licenseKey);

  if (result === ValidationType.VALID) {
    console.log('The Key is Valid.');
  } else {
    console.log(`The Key is invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```

### With Public Rsa Key
```js
//index.js
const NxGate = require('nxgate').default;
const { ValidationType } = require('nxgate');

async function checkLicense() {
  const userId = 'Your UserID';
  const licenseKey = 'Your LicenseKey';
  const publicRSAKey = 'Your RSA Key';

  const nxGate = new NxGate(userId).setPublicRsaKey(publicRSAKey).useChallenges(true);

  const result = await nxGate.verify(licenseKey);

  if (result === ValidationType.VALID) {
    console.log('The Key is Valid.');
  } else {
    console.log(`The Key is invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```