# etherpad-lite
A docker image for etherpad-lite using an alpine base image.

This image was largely inspired by [tvelocity/etherpad-lite](https://hub.docker.com/r/tvelocity/etherpad-lite/) but has some differences:

 - Based on alpine Linux (smaller images). The container has at the moment a size of 169 MB.
 - SESSIONKEY.txt and APIKEY.txt are stored in the var subdirectory and therefore are consistent when you rebuild your container.
	 - Also the APIKEY is not stored in settings.json, newer versions of etherpad-lite don't like this.

## Usage
Here is a usage example in docker-compose, if you don't use compose I'm confident you can figure it out:

```yml
version: '2'

services:
  mariadb:
      image: mariadb
      volumes:
        - ./mariadb:/var/lib/mysql
      environment:
        - MYSQL_ROOT_PASSWORD=<YOUR-PASSWORD>

  etherpad:
    image: fabianwe/etherpad-lite
    links:
      - "mariadb:mysql"
    expose:
      - "9001"
    environment:
      - ETHERPAD_DB_PASSWORD=<YOUR-PASSWORD>
    volumes:
      - ./etherpad:/opt/etherpad-lite/var
    ports:
      - "9001:9001"
```

You can change the following environment variables:

 - `ETHERPAD_DB_HOST` default "mysql"
 - `ETHERPAD_DB_USER` default "root"
 - `ETHERPAD_DB_PASSWORD` no default, must be set
 - `ETHERPAD_DB_NAME` default "etherpad"
 - `ETHERPAD_TITLE` default "Etherpad"
 - `ETHERPAD_PORT` default 9001
 - `ETHERPAD_ADMIN_USER` does not exist by default
 - `ETHERPAD_ADMIN_PASSWORD` does not exist by default

And of course change the generated settings.json.
