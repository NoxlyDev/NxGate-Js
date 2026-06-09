# NxGate JS Wrapper

JavaScript/Node.js wrapper for [NxGate](https://nxgate.noxlydev.xyz) license verification.

## Installation

```bash
npm install nxgate
```

## Dependencies

- Axios
- Node.js v20.11.0 or higher

---

## Usage

### Basic Verification
```js
const NxGate = require('nxgate').default;
const { ValidationType } = require('nxgate');

async function checkLicense() {
  const nxGate = new NxGate('YOUR_USER_ID');

  const result = await nxGate.verify('YOUR_LICENSE_KEY');

  if (result === ValidationType.VALID) {
    console.log('License is valid.');
  } else {
    console.log(`License invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```

---

### With Product Restriction
Verifikasi license hanya berlaku untuk product tertentu.
```js
const NxGate = require('nxgate').default;
const { ValidationType } = require('nxgate');

async function checkLicense() {
  const nxGate = new NxGate('YOUR_USER_ID');

  // Parameter: licenseKey, scope, metadata, productSlug
  const result = await nxGate.verify('YOUR_LICENSE_KEY', null, null, 'your-product-slug');

  if (result === ValidationType.VALID) {
    console.log('License valid untuk product ini.');
  } else if (result === ValidationType.PRODUCT_MISMATCH) {
    console.log('License tidak terdaftar untuk product ini.');
  } else {
    console.log(`License invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```

---

### With Scope + Product
```js
const nxGate = new NxGate('YOUR_USER_ID');

const result = await nxGate.verify('YOUR_LICENSE_KEY', 'MyScope', null, 'your-product-slug');
```

---

### With Custom Server URL
```js
const NxGate = require('nxgate').default;
const { ValidationType } = require('nxgate');

async function checkLicense() {
  const nxGate = new NxGate('YOUR_USER_ID')
    .setValidationServer('https://api.yourdomain.com');

  const result = await nxGate.verify('YOUR_LICENSE_KEY');

  if (result === ValidationType.VALID) {
    console.log('License is valid.');
  } else {
    console.log(`License invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```

---

### With RSA Challenge (Recommended for client-side)
```js
const NxGate = require('nxgate').default;
const { ValidationType } = require('nxgate');

async function checkLicense() {
  const nxGate = new NxGate('YOUR_USER_ID', 'YOUR_PUBLIC_RSA_KEY');

  const result = await nxGate.verify('YOUR_LICENSE_KEY');

  if (result === ValidationType.VALID) {
    console.log('License is valid.');
  } else {
    console.log(`License invalid. Reason: ${result}`);
  }
}

checkLicense().catch(console.error);
```

---

### verifySimple (returns boolean)
```js
const nxGate = new NxGate('YOUR_USER_ID');

const isValid = await nxGate.verifySimple('YOUR_LICENSE_KEY', null, null, 'your-product-slug');

if (isValid) {
  console.log('✅ Valid');
} else {
  console.log('❌ Invalid');
}
```

---

## ValidationType

| Value | Keterangan |
|---|---|
| `VALID` | License valid |
| `NOT_FOUND` | License tidak ditemukan |
| `NOT_ACTIVE` | License tidak aktif |
| `EXPIRED` | License sudah kadaluarsa |
| `LICENSE_SCOPE_FAILED` | Scope tidak cocok |
| `IP_LIMIT_EXCEEDED` | Batas IP terlampaui |
| `RATE_LIMIT_EXCEEDED` | Batas request terlampaui |
| `PRODUCT_MISMATCH` | License tidak terdaftar ke product ini |
| `FAILED_CHALLENGE` | Verifikasi RSA challenge gagal |
| `SERVER_ERROR` | Server mengembalikan response tidak valid |
| `CONNECTION_ERROR` | Gagal terhubung ke server |

---

## verify() Parameters

```ts
verify(licenseKey, scope?, metadata?, productSlug?): Promise<ValidationType>
```

| Parameter | Type | Keterangan |
|---|---|---|
| `licenseKey` | `string` | License key yang akan diverifikasi |
| `scope` | `string?` | Scope license (opsional) |
| `metadata` | `string?` | Metadata tambahan (opsional) |
| `productSlug` | `string?` | Slug product untuk Product Restriction (opsional) |
