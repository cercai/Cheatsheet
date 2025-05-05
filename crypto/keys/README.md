# Keys for client/server of SSH connections

## PEM keys

openssl is used to create key in PEM format
```
$ openssl genrsa -out .key_rsa 2048
```

## SSH keys

```cmd
ssh-keygen -t rsa  -b 4096 -f ~/.ssh/id_rsa
ssh-keygen -t ecdsa -b 521 -f ~/.ssh/id_ecdsa
ssh-keygen -t dsa -b 1024 -f ~/.ssh/id_dsa
```

Convert an ssh key into a pem key:
```cmd
ssh-keygen -p -m pem -f ~/.ssh/id_rsa
```

## PGP keys


