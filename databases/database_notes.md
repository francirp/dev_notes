Importing a database file from localhost to Heroku Postgres
-----------------------------------------------------------

**Step 1: Export Localhost Database**

pg_dump db_name > output_file

*note that database names can be found in database.yml

**Step 2: Put output_file on Amazon S3**

* Upload it to S3.
* Clone the following repo: https://github.com/dcartoon/s3-siggenerator
* Run open index.html
* Enter your S3 credentials
* Copy the canonical link provided by the tool

**Step 3: Import output_file into Postgres on Heroku**

heroku pgbackups:restore DATABASE_URL 'S3_LINK_FROM_PRIOR_STEP'

Postgres Settings:
------------------

in your ~/.bash_profile file:
```
export PGHOST=localhost
PATH="/Applications/Postgres.app/Contents/MacOS/bin:$PATH"
```

