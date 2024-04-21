## This is where your initial data for database locates

To have an intital database data for mysql container, you must put sample.sql file into this directory otherwise you can delete directory and below line on docker-compose.yml:

```
volumes:
 ./mysql/docker-entrypoint-initdb.d/:/docker-entrypoint-initdb.d
```
