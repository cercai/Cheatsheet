# PSQLsudo apt install postgresql-client-common

## Open and close a database server

Communicate with a database server

```cmd
$ psql -h <hostname> -p port -U user -d database
```

Quit the connection
```cmd
\q
```


## Build a database server

Building a postgre container
```
docker run --name my-postgres   -e POSTGRES_USER=admin   -e POSTGRES_PASSWORD=admin   -e POSTGRES_DB=mydb   -p 5432:5432   -d postgres
```

And connection to it
```cmd
$ psql -h localhost -p 5432 -U admin -d mydb
```

## Databases

List the databases
```cmd
\t
```

Select a database
```cmd
\c <database>
```

List of schemas
```cmd
\dn
```

List of users
```cmd
\du
```

List of tables in a database
```cmd
\dt
```

List of tables in a schema
```cmd
\dt schema.*
```


