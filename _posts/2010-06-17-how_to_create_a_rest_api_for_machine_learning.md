# How to create a microservice  with a REST API for machine learning models in Python

## A quick tutorial about Python Eve

You know how it is, if you a novice in any field you are impressed by the speed of experienced specialists. This was the
feeling that I had when I started my career as a machine learning engineer in 2017. While I was struggling with simplest models
the senior machine learning engineers crafted models in record time. However, with over 20 years of expirience as a software engineer
I was sometimes shocked how the same guys were not able to bring these models in a scalable way into production. For me this was the 
easy part. You might think this is not a task of a machine learning engineer, however having the knowledge is a plus and makes a difference.

In this little tutorial I want to show you how to bring a machine learning with a REST API into production in a microservice way. For this approach I am using
Python Eve which is a brilliant module for creating RESTful APIs with a bare minimum lines of code. The module is a bit like Spring Boot which is 
a game changer in the Java world. Python Eve uses MongoDB in the backend. Eve encapsulates the whole database access so you don't have to be scared about it. 
With the use of Docker containers and docker compose I reduce the effort of installing and setting up the database.

### 1. Create the RESTful API

[REST](https://en.wikipedia.org/wiki/Representational_state_transfer) stands for Representational state transfer. It defines constraints for creating Web Services. I don't want to pack to much theory into this tutorial, but I want to mention the key concept behind REST in one sentence, that is misunderstood by many people: REST uses resources