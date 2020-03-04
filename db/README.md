## Create MySQL image

### Requirements
- Run MySQL on docker container
- Expose port `3306`, connect from host via port `3333`.
- Store db data in host disk.

Build with tag
> docker build -t lab_db:0.1 .

Start Mysql and expose `3333` to host machine.
> docker run -d -p 3333:3306 lab_db:0.1 -name labdb

Where `-d` = detach; `-p` = expose port

To persist data of db to `datadir/db`
> docker run -d -p 3333:3306 --mount 'type=bind,source="<absolute_path>/datadir/db/",target=/var/lib/mysql' --name labdb_mount
lab_db:local

Where
- `--mount` = define volume
- `type` normally is
- `volume`, to mount host datadir, type should be `bind`.
- `source` require **absolute path** of host datadir
- `target` is path to container datadir

To inspect volume config
> docker inspect --format='{{json .Config.Volumes}}' labdb_mount