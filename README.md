# Generate a SSL certificate with SAN signed by Root CA
## 1. Generate RSA private key for the root certificate authority
`openssl genrsa -des3 -out rootCA.key 2048`
## 2. Generate certificate for the root certificate authority
`openssl req -x509 -new -nodes -key rootCA.key -days 825 -out rootCA.crt`
## 3. Generate RSA private key and Certificate Signing Request using a [CSR configuration file](csr.cnf) that specifies the Subject Alternative Names
`openssl req -new -nodes -sha256 -newkey rsa:2048 -keyout mydomain.key -out mydomain.csr -config csr.cnf`
## 4. Generate and sign the certificate
`openssl x509 -req -in mydomain.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out mydomain.crt -days 825 -sha256 -extensions req_ext -extfile csr.cnf`
## 5. Preview the newly made certificate
`openssl x509 -in mydomain.crt -noout -text`
## 6. Need a [PFX bundle](https://en.wikipedia.org/wiki/PKCS_12) to import into a server?
`openssl pkcs12 -export -out mydomain.pfx -inkey mydomain.key -in mydomain.crt`