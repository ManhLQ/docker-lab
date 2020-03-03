## Create MySQL image

### Requirements
- Run MySQL on docker container
- Expose port `3306`, connect from host via port `3333`.
- Store db data in host disk.

Build with tag
> docker build -t lab_db:0.1 .

Start Mysql and expose `3333` to host machine.
> docker run -d -p 3333:3306 lab_db:0.1 -name labdb