# xk6-sql-driver-ramsql

Database driver extension for [xk6-sql](https://github.com/grafana/xk6-sql) k6 extension to support RamSQL database.

## Example

```JavaScript file=examples/example.js
import sql from "k6/x/sql";
import driver from "k6/x/sql/driver/ramsql";

const db = sql.open(driver, "test_db");

export function setup() {
  db.exec(`
    CREATE TABLE IF NOT EXISTS roster
      (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        given_name VARCHAR NOT NULL,
        family_name VARCHAR NOT NULL
      );
  `);
}

export function teardown() {
  db.close();
}

export default function () {
  let result = db.exec(`
    INSERT INTO roster
      (given_name, family_name)
    VALUES
      ('Peter', 'Pan'),
      ('Wendy', 'Darling'),
      ('Tinker', 'Bell'),
      ('James', 'Hook');
  `);
  console.log(`${result.rowsAffected()} rows inserted`);

  let rows = db.query("SELECT * FROM roster WHERE given_name = $1;", "Peter");
  for (const row of rows) {
    console.log(`${row.family_name}, ${row.given_name}`);
  }
}
```

## Usage

Check the [xk6-sql documentation](https://github.com/grafana/xk6-sql) on how to use this database driver.

---

> [!IMPORTANT]
>
> ## TODO
>
> This is a repository template for creating an xk6-sql driver repository.
>
> After creating the driver repository, remember the following:
>
> - replace `RamSQL` with the database name in:
>   -  `README.md`
> - replace `ramsql` with the database driver name in:
>   - `README.md`
>   - `register.go`
>   - `register_test.go`
>   - `examples/example.js`
> - update SQL statements to match the database's SQL dialect in:
>   -  `testdata/script.js`
>   -  `examples/example.js`
>   -  `README.md`
> - change the go package and module name:
>   - `go.mod`
>   - `register.go`
>   - `register_test.go`
>   - `Makefile`
> - remove this alert blockquote from `README.md`

