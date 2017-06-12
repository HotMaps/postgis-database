# HotMaps-postgis-database Docker image

This Docker image offers PostgreSQL + PostGIS database.

## Software installed:
* PostgreSQL 9.6
* PostGIS 2.3

## Build and run:
### Build
To build this image from Dockerfile run this command in your Docker or Docker Toolbox shell:

`docker build -t hotmaps/postgis-database .`

### Run

To create a container use this command*:

`docker run --name postgis-database -e POSTGRES_USER=myuser -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_DB=toolboxdb –e PGDATA=`/path/to/custom/directory` -d hotmaps/postgis-database`

On Windows the absolute path to your code directory should be in the format */c/My-first-dir/my-second-dir/my-code-dir*

For more detailed instructions about how to start and control your Postgres container, see the documentation for the postgres image [here](https://registry.hub.docker.com/_/postgres/).

After successfuly running this command, open your web browser and go to 

    docker run -it --link postgis-database:toolboxdb --rm postgres \
        sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U myuser'

This image ensures that the default database created by the parent `postgres` image will have the following extensions installed:

* `postgis`
* `postgis_topology`
* `fuzzystrmatch`
* `postgis_tiger_geocoder`

Unless `-e POSTGRES_DB` is passed to the container at startup time, this database will be named after the admin user (either `postgres` or the user specified with `-e POSTGRES_USER`). If you would prefer to use the older template database mechanism for enabling PostGIS, the image also provides a PostGIS-enabled template database called `template_postgis`.

See [the PostGIS documentation](http://postgis.net/docs/postgis_installation.html#create_new_db_extensions) for more details on your options for creating and using a spatially-enabled database.

## Credits

**Special thanks** to the work done on the repository [https://github.com/appropriate/docker-postgis](https://github.com/appropriate/docker-postgis).

## Known Issues / Errors

When You encouter errors due to PostGIS update `OperationalError: could not access file "$libdir/postgis-X.X`, run:

`docker exec some-postgis update-postgis.sh`

It will update to Your newest PostGIS. Update is idempotent, so it won't hurt when You run it more than once, You will get notification like:

```
Updating PostGIS extensions template_postgis to X.X.X
NOTICE:  version "X.X.X" of extension "postgis" is already installed
NOTICE:  version "X.X.X" of extension "postgis_topology" is already installed
NOTICE:  version "X.X.X" of extension "postgis_tiger_geocoder" is already installed
ALTER EXTENSION
Updating PostGIS extensions docker to X.X.X
NOTICE:  version "X.X.X" of extension "postgis" is already installed
NOTICE:  version "X.X.X" of extension "postgis_topology" is already installed
NOTICE:  version "X.X.X" of extension "postgis_tiger_geocoder" is already installed
ALTER EXTENSION
```

