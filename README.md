# Readme

## Certificate

Our proxy will be an HTTPS server (--proto https), so we need certificate and private key. Let’s use self-signed certificate.

## Generate self-signed certificate

* cd certs
* ./gen.sh

## Add self-signed certificate as trusted to OS

### OSX
* https://tosbourn.com/getting-os-x-to-trust-self-signed-ssl-certificates/

### Linux
Copy your certificate in PEM format (the format that has ```----BEGIN CERTIFICATE----``` in it) into ```/usr/local/share/ca-certificates``` and name it with a ```.crt``` file extension.

Then run ```sudo update-ca-certificates```.



## Run Server
* cd server
* go run main.go

## Run Client

### Using curl
* cd certs
* curl -Lv --proxy https://localhost:8888 --proxy-cacert localhost.pem https://google.com

### Using go client
* cd client
* go run client.go