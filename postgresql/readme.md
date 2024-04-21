## This is where your persistent mysql data locates

If you don't bind the volumes for mysql and do not need persistent data, you must delete below line on docker-compose.yml

```
volumes:
  - ./mysql:/var/lib/mysql
```
