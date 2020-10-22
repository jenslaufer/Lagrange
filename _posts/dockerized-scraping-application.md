---
title: "Scraping Container with Docker"
output: html_document
---



My last article was about 10 lessons learned from scraping website. This is a more practicle hands on article. I want to show you a dockerized scraping conatiner with the use of the mulsti conatainer docker compose application. I am using my own container as a the base container for the application.

## File structure

```filesystem
./
Dockerfile
docker-compose.yml
requirements.txt
```


## Extended application container

You create first your container with your scraping code in. In the first line we we do a kind of import of a base image for our application. 
In this case you use my base image jenslaufer/docker-chromedriver-geckodriver. The image contains of the firefox with geckodriver and chrome with chromedriver. This image is based by itself uses the alpine linux system with python 3.7. 

```docker
FROM jenslaufer/docker-chromedriver-geckodriver:alpine-3.8-py3.7

RUN apk update && apk upgrade

RUN apk add make automake gcc g++ git libxml2-dev libxslt-dev

COPY requirements.txt requirements.txt 
RUN pip install -r requirements.txt
```

We install upgrade the apk installation manager apk and install some linux libraries. The librarieies are needed for pip to install some python modules. E.g libxml-dev2 and libxslt-dev are need for the lmxl module in python. The lxml is used for parsing the the html files.

The requirements.txt, which is on the same level like the dockerfile is "copied" into the docker image. Finally a pip install is executed with requirements.txt. So we get a docker image with everything we need.

We build the image with 

```shell
docker build -e docker-python3-scraper .
```


## The multi container app

Our application contains of our container with the base conatiner which is extended by a pip install, which installs all python modules we need. The second container is the mongodb container which we use to save our source files and the extraced meta information from the the source files. Let's dive into the application 

```yaml
version: '2'

services:
  report:
    image: docker-python3-scraper:latest
    container_name: "fivrscrp"
    volumes:
      - ./:/development
    depends_on:
      - mongodb
    entrypoint: /bin/sh
    stdin_open: true
    tty: true

  mongodb:
    image: mongo:3.6.3
    container_name: "mongodb"
    ports:
      - 27017:27017
    command: mongod

```


We can start the multi container app 
 ```code
 docker-compose up -d
 ```
 
 To stop the multi container app
```code
docker-compose stop
```


We can open a shell in the container with

``` shell
docker exec -it fivrscrp /bin/sh
```
Now  you are in the container and you can excute scripts like you want.
 
