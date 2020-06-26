# How to create a microservice  with a REST API for machine learning models in Python

## A quick tutorial about Python Eve

You know how it is, if you a novice in any field you are impressed by the speed of experienced specialists. This was the
feeling that I had when I started my career as a machine learning engineer in 2017. While I was struggling with simplest models
the senior machine learning engineers crafted models in record time. However, with over 20 years of expirience as a software engineer
I was sometimes shocked how the same guys were not able to bring these models in a scalable way into production. For me this was the 
easy part. You might think this is not a task of a machine learning engineer, however having the knowledge is a plus and makes a difference, as 
there not always software enigneers in the data science team.

In this little tutorial __I want to show you how to create a microservice with a REST API for a machine learning model__. For this approach I am using
Python Eve which is a brilliant module for creating RESTful APIs with a bare minimum lines of code. The module is a bit like Spring Boot which is a game changer in the Java world. Python Eve uses MongoDB in the backend. Eve encapsulates the whole database access so you don't have to be scared about it.
With the use of Docker containers and docker compose I reduce the effort of installing and setting up the database.

In our example we create a model that maps the BSR (Best seller rank) of a product on Amazon into an estimation of monthly sales.

### 1. Create the RESTful API

[REST](https://en.wikipedia.org/wiki/Representational_state_transfer) stands for Representational state transfer. It defines constraints for creating Web Services. I don't want to pack too much theory into this tutorial, but I want to mention the key concept behind REST in a few sentences, that is misunderstood by many people: A REST API uses stateless resources, which can be manipulated with standard HTTP methods POST, PUT, GET and DELETE. This is different to a API around [Remote
Procedure Call (RPC)](https://en.wikipedia.org/wiki/Remote_procedure_call), which uses functions/procedures, that you can call remotely. In case you have shopping cart in REST API you have a resource "Order", in a RPC API you have functions e.g. "createOrder". REST APIs focus on nouns (Order) and RPC APIs on verbs (create). I have seen many people that create REST APIs which are actually RPC APIs.

In case you create a REST API by "hand" you have to keep the concept around REST in mind. However, the good news are that Eve is creating the API with all endpoints for you according the specification of REST. You can concentrate on Machine Learning instead of the concept around REST.

So let's get the hands dirty. Our resource we want to to use is called "prediction". With a POST request a "prediction" resource is created in the database. It's not just created, a prediction is also performed. With a GET request we can get the prediction "resource", with a PUT we can change it and DELETE removes it. To setup the the API we need a settings.py 

```python
import os
import pymongo

# Fetch mongouri fro environent, otherwise use localhost
MONGO_URI = os.environ.get("MONGO_URI", 'mongodb://localhost/estimation')


RESOURCE_METHODS = ['GET', 'POST']

ITEM_METHODS = ['GET', 'DELETE']

CACHE_CONTROL = 'max-age=20'
CACHE_EXPIRES = 20

X_DOMAINS = '*'
X_HEADERS = 'Content-Type,Authorization'


estimations = {
    'item_title': 'estimations',
    'schema': {
        'bestseller_rank': {
            'type': 'integer',
            'required': True
        },
        'category': {
            'type': 'string',
            'required': True
        },
        'estimated_sales': {
            'type': 'integer'
        }
    }}


DOMAIN = {
    'estimations': estimations
}

```

To start our REST API we have another python file app.py with just a few lines of code:

```python
from eve import Eve

app = Eve()
app.run()

```

You can execute the app.py now and your REST API is ready to use. Almost. We missed something very essential:

__The MongoDB database__

Installing a database that you don't know might be nightmare. However, we reduce the effort with the use docker-compose and Docker.

### 2. Containerize your Application with Docker

The usage of docker-compose is a way to start an application that consists of multiple sub-applications like services, frontends, databases.
The docker-compose.yml file wraps up the application. Please ensure that you have [Docker](https://docs.docker.com/get-docker/) and [docker-compose](https://docs.docker.com/compose/install/) installed on your machine for the next steps.

The first step is to build a container for your REST API. It's easy, because you just need Dockerfile for it. There are just a few commands in the Dockerfile, that are easy to understand in case you are familiar with Linux/MacOS. 

```Dockerfile
# The base image we use
FROM python:3.7

# We copy the resources that are need for the application
COPY requirements.txt requirements.txt
COPY app.py app.py
COPY settings.py settings.py

# We need to install the requirements into the the container
RUN pip install --upgrade pip && pip install -r requirements.txt

CMD [ "python", "app.py" ]
```

You can build the docker container from the command line. However, I skip it, because I let docker-compose do the building of the container to keep this tutorial simple.

Like I mentioned before, we need a a docker-compose.ym file. Let's take look on it.

```docker-compose
version: "3.7"

services:
  sales-estimator-backend:
    image: sales-estimator-backend:latest
    container_name: sales-estimator-backend
    ports:
      - 5000:5000
    depends_on:
        - sales-estimator-db
    environment:
      - MONGO_URI=mongodb://sales-estimator-db/estimations
    build: .

  sales-estimator-db:
    image: mongo:4.0
    container_name: sales-estimator-db
    ports:
      - 27017
```

Your application is consisting of two containers:

- sales-predictor: Your REST Service
- predictor-db: The MongoDB for your REST Service

We can start our application with a docker-compose command in the directory where the 
Dockerfile and the docker-compose.yml is located:

```shell
docker-compose up -d
```

What exactly happens when you execute this command?

- sales-predictor: Docker builds your container and starts it.
- predictor-db: Docker fetches the image for MongoDB from the [Dockerhub registry](https://hub.docker.com/_/mongo) and starts it.

Especially the "set up" of the database is a no-brainer. Exactly what you need, as you want to concentrate on Data Science and not database administration.

You can now create a prediction resource with a POST request. You can use a REST client for it:
I recommend the [Advanced REST client](https://install.advancedrestclient.com/install), which is very is easy to use. You can also get a Chrome plugin for it. [Postman](https://www.postman.com/product/api-client/) has much more features, however it's a bit more complicated.

Let's create a prediction for a bestseller rank of 70. We send a POST request with a JSON body:

```json
{
  "bestseller_rank": 70
}
```

We get a HTTP status 201 back, which means "CREATED" and information about the created resource:

```json
{
    "_updated": "Thu, 25 Jun 2020 06:42:02 GMT",
    "_created": "Thu, 25 Jun 2020 06:42:02 GMT",
    "_etag": "ed58e79f053c76083372af1d3f48393c924fd824",
    "_id": "5ef4473b16d99e36701ba03b",
    "_links": {
        "self": {
            "title": "suggestions",
            "href": "predictions/5ef4473b16d99e36701ba03b"
        }
    },
    "_status": "OK"
}
```

Python Eve added also valuable information like the timestamp the resource was created and updated.


You can now fire a GET request with the id "5ef4473b16d99e36701ba03b" do get all "content" of our resource. The url for this request is: http://localhost:5000/predictions/5ef4473b16d99e36701ba03b

We get a HTTP 200 back a JSON response:

```json
{
    "_id": "5ef4473b16d99e36701ba03b",
    "bestseller_rank": 70,
    "_updated": "Thu, 25 Jun 2020 06:42:02 GMT",
    "_created": "Thu, 25 Jun 2020 06:42:02 GMT",
    "_etag": "ed58e79f053c76083372af1d3f48393c924fd824",
    "_links": {
        "self": {
            "title": "suggestions",
            "href": "predictions/5ef4473b16d99e36701ba03b"
        },
        "parent": {
            "title": "home",
            "href": "/"
        },
        "collection": {
            "title": "predictions",
            "href": "predictions"
        }
    }
}
```

The whole REST API is according to specification of REST with correct HTTP codes. Thanks to Eve!

However, there is still something missing: 

__Prediting__

### 3. Integration of the predictor

In a different git repository I created a predictor. It uses data from
