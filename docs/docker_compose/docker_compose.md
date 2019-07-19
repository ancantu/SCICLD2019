Each container provides a single service. Can be written in different lanugees but need to be able to communicate between containers. Need to specify ports and mount volumes, it can get pretty tedious as the number of containers increases. Docker compose lets us define all of our services in a config file (yaml), and lets us spin up all of our containers at the same time. Create a dirctory on dir up `docker-compose.yml`, version of compose format (3).

```
version: '3'
service:
  product-service:
    build: ./product
    volumes:
     - ./product:/src/app/
    ports:
     - 5001:80
  other-service:
    image: jurrutia/app:tag
    volumes:
     - ./local:/continer/loc
    ports:
     - 5000:80
    depends_on:
     - product-service
```

```
docker-compose up
```

Compose creates a virtual network for all of the containers, so any container in the compose file can access any other container.
Defining and creating multi container applications

To find out which version of docker compose you have installed, you can run:
```
train199@js-17-119:~$ docker-compose -v
docker-compose version 1.17.1, build unknown
```
```
docker-compose version
```

```
train199@js-17-119:~$ docker-compose version
docker-compose version 1.17.1, build unknown
docker-py version: 2.5.1
CPython version: 2.7.15+
OpenSSL version: OpenSSL 1.1.1  11 Sep 2018
train199@js-17-119:~$
```
