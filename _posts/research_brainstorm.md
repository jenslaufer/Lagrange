---
title: "research & brainstorm"
output: html_document
---



The 5 uses cases of docker in data science

docker in data science

How to improve your data science skills with docker

How to save time in data science with docker

How to boost your data sciece process with docker
















=====================================================0
Everybody you does something with computer knows this saying. "but it works on my computer".

(I could be a little comic)

We had it most probably fro both sites. "well you forgot to install the blabla lib". 
"i have it" 
"oh"
"you need version 2.3.7"
"o i have 2.2.1"
"still not working"

We waste a lot of time with. Docker minimalizes this problem as you deliver the application in a conatiner with verything is needed. You get __reproducable__ runtime environment

Problem with installed libraries (e.g. requirements.txt, ). You ship a juypter notebook and instalation instructions in natural langugage
.The problem with natural language is that people don't understand it or miss a step. Things get even woth in case we need operation system specific installations.
With docker you ship a conatiner which everything what is needed for the special use case. You can run the container as it is (no instlallation)
You have that with python virtual environments or anaconda. But maybe you need it more lightweight or you need avery weird enviroment.
You can build containers with the weirdes environment. You can put container (similiar to libs) to repositories. The future might be polyglot, which Dokcer can easily handle
It come with it's price (container get easly very big)


## What is Docker?

Docker is a lightweight container for packing applications together

- Advantages over other virtualizations techniques like Python virtual environments or anaconda or virtual OS. 
- You have one virtualization technique for everything
- What differs it to these platforms
- Avoid the "It works on may computer"
- Reliable enviroment
- Exkurs on Kubernetes (make your application easily scalabe)


Research on what it differs to other virtualization platforms

## Simple example of container

Example of simple scraping container


## Docker compose for multiple container applications:

With docker compose you are able to pack more complex multi container applcations.


## Use case of Docker for Data Science



- Container for scraping data
- Container for Data Product like Shiny App
- Container with a static web site
- Container for training in machine learning training pipeline
- Conatiner wit a trained model





## Research

[https://towardsdatascience.com/docker-for-data-scientists-5732501f0ba4](Docker for Data Scientists):

What is Docker?


Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.

It’s a software container platform that provides an isolated container for us to have everything we need for our experiments to run. Essentially, it’s a light-weight VM that’s built from a script that can be version controlled; so we can now version control our data science environment! Developers use Docker when collaborating on code with coworkers and they also use it to build agile software delivery pipelines to ship new features faster. Any of this sound familiar?

- “Not sure why it’s not working on your computer, it’s working on mine.”
- “It’s a pain to install everything from scratch for Linux, Windows, and MacOS, and trying to build the same environment for each OS.”
- “Can’t install the package that you used, can you help me out?”
- “I need more compute power. I could use AWS but it’ll take so long just to install all those packages and configure settings just like I have it on my machine.”


[Docker for Data Science](https://www.kdnuggets.com/2018/01/docker-data-science.html):

-  simplifies the installation process for software engineers

What’s in it for Data Scientists?


Portability, Reproducability

1. Time: The amount of time that you save on not installing packages in itself makes this framework worth it.

2. Reproducible Research: I think of Docker as akin to setting the random number seed in a report. The same dependencies and versions of libraries that was used in your machine are used on the other person’s machine. This makes sure that the analysis that you are generating will run on any other analysts machine.

3. Distribution: Not only are you distributing your code, but you are also distributing the environment in which that code was run.

How Does it Work?

Docker employs the concept of (reusable) layers. So whatever line that you write inside the Dockerfile is considered a layer. For example you would usually start with:

FROM ubuntu
RUN apt-get install python3
 

This Dockerfile would install python3 (as a layer) on top of the Ubuntu layer.

What you essentially do is for each project you write all the apt-get install, pip install etc. commands into your Dockerfile instead of executing it locally.

I recommend reading the tutorial on https://docs.docker.com/get-started/ to get started on Docker. The learning curve is minimal (2 days work at most) and the gains are enormous.
