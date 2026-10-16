# S/MIME Generator

A single-page browser app that creates an RSA key pair and a self-signed S/MIME certificate for email signing and encryption, and packages them as a password-protected `.p12` file.

All cryptography runs locally: keys are generated with Web Crypto and the certificate and PKCS#12 file are built with [node-forge](https://github.com/digitalbazaar/forge). Nothing is sent to a server.

## Usage

Open `index.html` in a browser (as a local file, from `localhost`, or over HTTPS — Web Crypto is unavailable on plain HTTP), fill in the form, and download:

- **`.p12`**: private key + certificate, to import into eM Client, Outlook, Thunderbird or Apple Mail.
- **`.cer`**: public certificate only, for recipients who want to trust it explicitly.

## Certificate profile

- RSA 2048 / 3072 / 4096, SHA-256 signature, random 128-bit serial
- Subject: `CN`, optional `O`, `emailAddress`; SAN `rfc822Name`
- Basic Constraints `CA:FALSE` (critical), Key Usage `digitalSignature, nonRepudiation, keyEncipherment` (critical), Extended Key Usage `emailProtection`
- `.p12` encryption: AES-256 (PBES2) or 3DES for older clients

Because the certificate is self-signed, recipients see an untrusted-issuer warning until they trust the `.cer` file manually.
