## Create self-signed certificate
```sh
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes \
-keyout server.key -out server.crt -subj "/CN=example.com" \
-addext "subjectAltName=DNS:example.com,DNS:www.example.net,IP:10.0.0.1"
```

## View certificate and key
Certificate
```sh
openssl x509 -noout -text -in server.crt
```
Key
```sh
openssl rsa -noout -text -in server.key
```

## View certificate date
start and end
```sh
openssl x509 -noout -dates -in server.crt
```
start only
```sh
openssl x509 -noout -startdate -in server.crt
```
end only
```sh
openssl x509 -noout -enddate -in server.crt
```

## Check whether the certificate expires in the next day (86400 seconds)
```sh
openssl x509 -checkend 86400 -noout -in server.crt
```

## Check that certificate match the key
The 'modulus' and the 'public exponent' portions in the key and the Certificate must match. As the public exponent is usually 65537 and it's difficult to visually check that the long modulus numbers are the same, you can use the following approach:
```sh
openssl x509 -noout -modulus -in server.crt | openssl md5 && \
openssl rsa -noout -modulus -in server.key | openssl md5
```


A simple way to compare two 'modulus' (If more than one hash is displayed, they don't match):
```sh
(openssl x509 -noout -modulus -in server.pem | openssl md5; openssl rsa -noout -modulus -in server.key | openssl md5) | uniq
```

## Check to which key or certificate a particular CSR belongs
```sh
openssl req -noout -modulus -in server.csr | openssl md5
```

## View remote certificate
```sh
openssl s_client -connect your_host.domain.com:443 | openssl x509 -noout -text
```
