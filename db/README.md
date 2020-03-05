## Create MySQL image

### Requirements
- Run MySQL on docker container
- Expose port `3306`, connect from host via port `3333`.
- Store db data in host disk.

#### Build with tag
```bash
$ docker build -t lab_db:0.1 .
```

#### Start Mysql and expose `3333` to host machine.

```bash
$ docker run -d -p 3333:3306 lab_db:0.1 -name labdb
```

Where `-d` = detach; `-p` = expose port

#### Persisting data
- Using `volume`

Create volume
```bash
$ docker volume create lab_db_vol
```
Mount volume to container
```bash
$ docker run -d -p 3333:3306 --mount type=volume,source=lab_db_vol,target=/var/lib/mysql --name labdb lab_db:0.1
```

- Using `bind mount` to persist data at `datadir/db`
```bash
$ docker run -d -p 3333:3306 --mount type=bind,source="$(pwd)"/datadir/db/,target=/var/lib/mysql --name labdb lab_db:0.1
```

Where
- `--mount` = define volume
- `type` normally is `volume`, to mount host datadir, type should be `bind`.
- `source` require **absolute path** of host datadir
- `target` is path to container datadir

Create a sample table and insert sample data
```sql
CREATE TABLE `sample` (
  `id` int NOT NULL,
  `content` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB;
```

```sql
INSERT INTO sample VALUES (1, 'test1'), (2, 'test 2');
```

Then stop and delete container
```bash
$ docker container stop labdb && docker container rm labdb
```

Create another container using same volume, data should be exist!

To inspect volume config
```bash
$ docker inspect --format='{{json .Config.Volumes}}' labdb_mount
```

