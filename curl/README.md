# CURL

## Documentation

Refer to this documentation if you have any question:
`https://curl.haxx.se/book.html`

## Examples

It is possible to get the body of a page or the header or both:
```sh
curl www.example.com
<BODY>

curl -i www.example.com
<HEADER>
<BODY>

curl -I www.example.com
<HEADER>
```

Use `-L` or `--location` to follow a redirection.

```sh
curl -I -L www.example.com
<HEADER 1>
<HEADER 2>
```

## Combinations

Send several requests at once
```sh
# SENDS 9 REQUESTS
curl www.example.com/file[1-9].txt
# REQUESTS ODD TXT FILES
curl www.example.com/file[1-9:2].txt  # 1.txt, 3.txt, 5.txt ..
# BIGGER REQUEST
curl www.example.com/file[01-99].txt
# WITH LETTERS
curl www.example.com/file[a-z].txt
# a.txt, d.txt, g.txt ...
curl www.example.com/file[a-z:3].txt
# SAVE IN DIFFERENT FILES
curl www.example.com/file[1-9].txt -o save_#1.txt
curl www.example.com/{toto,titi,tata} -o file_#1.jpg
curl www.example.com/{toto,titi,tata}/issue[012-132]
```

## Verbose

If you need some verbose to see TLS handshake etc.
```sh
# Adding more v is useless
curl -v www.example.com
```

## Headers

```sh
curl -H "Content-Type: application/json" www.example.com
# Remove User-Agent in the header
curl -H "User-Agent:" www.example.com
# Leave it blank
curl -H "User-Agent;" www.example.com
```

## Send data

```sh
curl -d name=toto www.example.com
curl -d @file www.example.com
```

## Cookies

```sh
curl -c cookie.txt www.example.com/login
curl -b cookie.txt -c cookie.txt -d name=toto -d password=1234 www.example.com/home

curl -b cookies.txt -b "additional_param=new_value" https://example.com/dashboard
```

## Ignore the certificate
```sh
curl -k https://127.0.0.1/
```

## Add a certificate

Tells curl to use a specific certificate to verify the peer.
```
curl --cacert <certificate> https://127.0.0.1/
```


## Fetch specific parameter
```sh
curl -s -o file.save -w "%{json}" https://example.com | jq
curl -s -o /dev/null -w "%{http_code}" https://example.com
```

