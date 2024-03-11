---
description: create key
---

# SSL - generate key

```
Download  openssl

// Some code
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365

PEM pass phrase: 
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:
Email Address []:

>>key.pem and cert.pem

openssl rsa -in key.pem -out key-rsa.pem

phrase for key.pem: (same above)

>>key-rsa.pem


ref: 
https://nodejs.org/en/knowledge/HTTP/servers/how-to-create-a-HTTPS-server/
https://stackoverflow.com/questions/11744975/enabling-https-on-express-js

https://devopscube.com/create-self-signed-certificates-openssl/
https://www.baeldung.com/openssl-self-signed-cert
```
