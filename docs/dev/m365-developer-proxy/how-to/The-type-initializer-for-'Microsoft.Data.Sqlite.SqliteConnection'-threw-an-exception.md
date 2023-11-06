After starting the proxy you may encounter an error thrown.

```
The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception.
```

This happens when the `libe_sqlite3.dylib` file has not been made an executable.

Open the `m365-developer-proxy` installation folder in a terminal and execute `chmod -x libe_sqlite3.dylib`.
