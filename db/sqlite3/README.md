# SQLITE3

## Open and close a database file

```cmd
$ sqlite3 grafana.db
```

```cmd
$ sqlite3
sqlite> .open grafana.db
```

To exit, type `.exit` or `.quit`.

## Navigate between databases, tables and schemas

To list databases
```cmd
sqlite> .databases
```

To list tables
```cmd
sqlite> .tables
```

To describe the table
```cmd
sqlite> .schema table
sqlite> PRAGMA table_info(table);
```

