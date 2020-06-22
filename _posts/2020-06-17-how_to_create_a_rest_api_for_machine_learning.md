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
# -*- coding: utf-8 -*-

# URI of our database
MONGO_URI = 'mongodb://localhost/prediction'

# What methods we are able to perform on resources
RESOURCE_METHODS = ['GET', 'POST']

# What methods we are able to perform on items
ITEM_METHODS = ['GET', 'DELETE']

CACHE_CONTROL = 'max-age=20'
CACHE_EXPIRES = 20

X_DOMAINS = '*'
X_HEADERS = 'Content-Type,Authorization'

# The schema of our 
predictions = {
    'item_title': 'suggestions',
    'schema': {
        'bsr': {
            'type': 'int',
            'required': True
        },

        'sales': {
            'type': 'int'
        }
    }}


DOMAIN = {
    'predictions': predictions
}
```

To start our REST API we have another python file app.py with just a few lines of code:

```python
from eve import Eve

app = Eve()
app.run()

```
