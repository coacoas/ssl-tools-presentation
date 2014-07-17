# HTTPS & SSL (& certificates (& openssl (& keytool)))
This is a high level introduction to how SSL works, and how to work with certificates and keystores.  It covers:
- encryption
- HTTPS/SSL
- openssl to generate certificates
- keytool to work with Java Keystores

## Commands covered
### Generate a self-signed certificate
    keytool -keysize 2048 -genkey -alias <alias> -keyalg RSA -keystore <keystore.jks>

### Generate a self-signed certificate with v3 extensions
    openssl -genrsa -des3 -out <keyfile> 2048
    openssl req -new -key <keyfile> -out <csrfile>
    openssl x509 -req -days 3650 -in <csrfile> -inkey <keyfile> -out bg.crt [-extfile <extensions]

### Extensions file (valid values described at https://www.openssl.org/docs/apps/x509v3_config.html):
    basicConstraints=CA:FALSE
    keyUsage=digitalSignature,keyCertSign
    extendedKeyUsage=serverAuth,clientAuth
    authorityKeyIdentifier=keyid,issuer
    subjectAltName=URI:bridgegatetest.com,URI:*.bridgegatetest.com

### Convert from DER to PEM
    openssl x509 -in <file> -inform DER -out <target> -outform PEM

### Import a certificate to a keystore
    keytool -importcert -keystore <filename> -file <public cert filename> \
    [-alias <alias>]
This can be done using the BridgeGate SSL Certificate Manager in the Workbench.

### Import a public/private key pair to a keystore
    openssl pkcs12 -in <certificate> -inkey <key> -export -out <keystore.p12>
    keytool -importkeystore -srckeystore <keystore.p12> -srcstoretype pkcs12 -destkeystore <keystore.jks> -srcalias <p12 alias> -destalias <jks alias>


- - -
Uses [reveal.js](http://lab.hakim.se/reveal-js) to generate the presentation.
