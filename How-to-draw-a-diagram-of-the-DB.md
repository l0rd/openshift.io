### How to draw a diagram of the DB
* `brew install graphviz`
* `brew install postgres`
* Download https://jdbc.postgresql.org/download/postgresql-42.1.1.jar
* Download https://sourceforge.net/projects/schemaspy/files/schemaspy/SchemaSpy%205.0.0/schemaSpy_5.0.0.jar/download?use_mirror=kent&r=https%3A%2F%2Fsourceforge.net%2Fprojects%2Fschemaspy%2Ffiles%2Fschemaspy%2F&use_mirror=kent 
* Copy both jar in current directory
```
 java -jar schemaSpy_5.0.0.jar -t pgsql -db postgres -host 127.0.0.1 -u postgres -p mysecretpassword -o ./schemaspy -dp postgresql-42.1.1.jar -s public -noads
```