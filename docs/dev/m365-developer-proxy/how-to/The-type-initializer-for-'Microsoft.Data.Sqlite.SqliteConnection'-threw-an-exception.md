After starting the proxy, you see the following error:

```text
The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception.
```

This error happens when macOS doesn't consider the `libe_sqlite3.dylib` file an executable.

Open the `m365-developer-proxy` installation folder in a terminal and execute `chmod -x libe_sqlite3.dylib`.
