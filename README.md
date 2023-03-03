# Docker Compose - Infinispan + Oracle

This is a sample setup with Infinispan 14 and Oracle 21c XE set up as Infinispan's JDBC storage

# Before you run it

Before you run the docker compose command please initialize the project:

```shell
gradlew dockerInit
```

It will download the Oracle JDBC jar for you (from the Maven Central) placing it to
the `vshop-docker/infinispan/server/lib`. This folder is dedicated to the Infinispan 3rd party libraries. Read more
about the Infinispan `server`
folder https://infinispan.org/docs/stable/titles/server/server.html#server_root_directory

## Start the infrastructure

```shell
docker-compose up -d
```

It will start Oracle 21c XE instance and Infinispan 14 (connected to the Oracle DB).

### Oracle

Oracle creates required users on db initialization. The following script is run only once (when Oracle DB is created): 
[SQL init scripts](./vshop-docker/oracle/scripts/setup)

One of the created users `vshopcache` is used by Infinispan.\
Read more about the [Initialization scripts](https://github.com/oracle/docker-images/tree/main/OracleDatabase/SingleInstance#running-scripts-after-setup-and-on-startup) 

Try to connect to the Oracle console:\
https://localhost:5500/em \
user/password: `sys/sys`

### Infinispan

Infinispan has configured JDBC connection (using `vshopcache` user) to persist the cache to the Oracle instance. 

`countries` cache is set up at start (see [Cache config](./infinispan/server/conf/infinispan.xml))

Try to connect to the Infinispan console:\
http://localhost:11222/console/ \
user/password: `vshop/vshop` \
container name: leave empty or type `CDB$ROOT`

# How to

### How can I connect to the Oracle DB

You can download the [DBaver SQL IDE](https://dbeaver.io/) (I prefer via `scoop`) and use the following JDBC connection:
```
jdbc:oracle:thin:@//localhost:1521/xepdb1
```

**user/password configured:**\
sys as sysdba/sys \
vshop/vshop \
vshopcache/vshopcache (here you will find `cache_` tables created by Infinispan)
