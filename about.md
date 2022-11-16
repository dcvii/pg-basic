### abstract

this is an attempt to create a basic postgres database for when i need one. Other posgresql DBs will run out of the high level devcode/PG

suivez la model: https://stackoverflow.com/questions/56643961/initialize-postgres-db-in-docker-compose


I managed to make it work using custom Dockerfile, here's my solution:

Project structure
data/
  datasource.csv
db/
  scripts/
    1_init.sql
    2_copy.sql
  Dockerfile
docker-compose.yml
Files
CSV file is located in data folder inside of the project.

In the project folder there is the following docker-compose.yml file:

version: '3.3'

services:
  db:
    build: ./db
    container_name: postgres
    ports:
      - "5431:6666"
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=db_name
    volumes:
      - ./data:/data
Dockerfile contains:

FROM postgres:alpine
ADD scripts/1_init.sql /docker-entrypoint-initdb.d
ADD scripts/2_copy.sql /docker-entrypoint-initdb.d
RUN chmod a+r /docker-entrypoint-initdb.d/*
EXPOSE 6666
1_init.sql body:

CREATE TABLE table_name
(
   --statement body
);
And 2_copy.sql:

COPY table_name FROM '/data/datasource.csv' DELIMITER ',' CSV HEADER;
Explanation
1_init.sql creates the DB table, it has to have the same column names as in CSV file. 2_copy.sql is responsible for copying data from the CSV to postgres.

Dockerfile uses postgres image and copies all *.sql files to /docker-entrypoint-initdb.d/. Later, all files are executed in alphanumerical order, that's why *.sql files start with digits. Finally, port 6666 is exposed.

docker-compose.yml builds the Dockerfile from db folder and make it accessible through 5431 port. As environmental properties basic postgres properties are used. And at the end data folder with CSV file is copied to the container.


---

Also see this YouTube (https://www.youtube.com/watch?v=eKEzq59FhEw)

--mdcb 2022-11-18