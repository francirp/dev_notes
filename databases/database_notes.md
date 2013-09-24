
**Postgres**

To import a seed file:
```
psql db_name < import_file
```

To dump a seed file:
```
pg_dump db_name < output_file
```
*note that database names can be found in database.yml

in your ~/.bash_profile file:
```
export PGHOST=localhost
PATH="/Applications/Postgres.app/Contents/MacOS/bin:$PATH"
```
