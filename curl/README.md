# CURL

```sh
curl www.example.com
<BODY>

curl -i www.example.com
<HEADER>
<BODY>

curl -I www.example.com
<HEADER>


curl -I -L www.example.com
<HEADER 1>
<HEADER 2>
...

curl www.example.com/file[1-9].txt
curl www.example.com/file[1-9:2].txt  # 1.txt, 3.txt, 5.txt ..
curl www.example.com/file[01-99].txt
curl www.example.com/file[a-z].txt
curl www.example.com/file[a-z:3].txt  # a.txt, d.txt, g.txt ..

curl www.example.com/file[1-9].txt -o save_#1.txt  # files: save_1.txt, save_2.txt, etc..
curl www.example.com/{toto,titi,tata} -o file_#1.jpg
curl www.example.com/{toto,titi,tata}/issue[012-132]

# For verbose
curl -v www.example.com
# Adding more v is useless

curl -H "Content-Type: application/json" www.example.com
# Remove User-Agent in the header
curl -H "User-Agent:" www.example.com
# Leave it blank
curl -H "User-Agent;" www.example.com

curl -d name=daniel www.example.com
curl -d @file www.example.com
curl -d name=daniel www.example.com

```




curl -b cookies.txt -b "additional_param=new_value" https://example.com/dashboard





