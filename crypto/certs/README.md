# Certificates

## Creating certificates for client/servers

### Create the Public Key Infrastructure
First, we need to create a Public Key Infrastructure (PKI).
We create the private pem key
```cmd
$ openssl genpkey -algorithm RSA -out private/ca.key -aes256
```
Enter a password then confirm it.
After what, create the self-signed certificate
```cmd
$ openssl req -new -x509 -days 3650 -key private/ca.key -out certs/ca.crt
```
You will be asked to answer some questions.



Openssl is used to create certificates for client/servers.
There are two common methods to create the Certificate Signing Request (CSR)

### First method
First you need to create a key in PEM format
```cmd
$ openssl genrsa -out <key> 4096
```

Then you need to create a CSR by answering some prompted questions.
```cmd
$ openssl req -new -key <key> -out <csr>
```

### Second method

The csr can be generated from a config file with all information from questions asked in the first method. The key and the csr will be created in the following command:
```cmd
$ openssl req -new -nodes -keyout <key> -out <csr> -config <cnf>
```

the `cnf` file is a config file that should have the following template:
```cnf
[req]
default_bits = 4096
distinguished_name = dn
prompt = no
default_md = sha256
req_extensions = req_ext

[dn]
C = {{country}}
ST = {{state}}
L = {{locality}}
O = {{organization}}
OU = {{organizationalUnit}}
emailAddress = {{email}}
CN = {{FQDN}}

[req_ext]
subjectAltName = @alt_names

[alt_names]
DNS.0 = {{hostname1}}
DNS.1 = {{hostname2}}
```

## Read information from csr
To make sure the CSR is correct, you can extract information from it with the following command
```cmd
$ openssl req -in <csr> -text -noout
```

## Read information from certificate
To extract information from the x509 certificate, use the following command
```cmd
$ openssl x509 -in <cer> -text -noout
```

## Check the private key and the certificate match

To verify the key correspond to the certificate, make sure both following commands output the same result:
```cmd
$ openssl rsa -modulus -noout -in <key> | openssl md5
$ openssl x509 -modulus -noout -in <cer> | openssl md5
```


