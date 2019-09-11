---
title: "qraptor Technology Stack Part Two: Applications"
subtitle: "The application we use for our reinforcement trading platform"
output: html_document
image: "https://res.cloudinary.com/jenslaufer/image/upload/c_scale,q_74,w_800/v1568015884/matt-artz-pH6wLT6TVFc-unsplash.jpg"
image_caption: "Photo by Matt Artz on Unsplash"
tags: docker, programming languages, technology stacks, applications
categories: qraptor
---

In [Part One: Architecture and Programming Languages](/qraptor/qraptors-technology-stack-programming-languages.html) we showed you that we using several programming 
languages to get things done. Today we want to go more into details what applications/micro services for our reinforcement trading platform. It's devided into
several parts. First, we show you the shared technical services we use and second all the services to run our core business: algorithmic trdaing with reinforcement learning.

## Shared Technical Services ##

## MongoDB ##

![MongoDB Logo](@TODO)

MongoDb is our persistence container for a variety of artefacts, some of them are saved as MongoDB documents, which is basically simple JSON. We use also the GridFS-Filesystem for persisting
file-like structures. We love MongoDB, because of it's simplicity. You don't need schemas to persist data. However, we know that having no schema is in some situations risky.

The artefacts we persist in MongoDB:

- Training Session Meta Data 
@TODO example

We save the complete the Meta Data for each training session with all the parmeters in the MongoDB. The trainer fetches new training sessions and dynamically initializes the train

- Trained Models from Trainer in hdf5 file

What we haven't done so far is to persist the source data files into the MongoDB, although they should be there to be fully . To be honest we are bit to scared to save hundreds of GB in the GridFS.
filesystem.

- Training results as CSV


## Portainer ##

Every application/service we use is packed as a Docker container. We use docker-compose and docker-machine for the management of the application. We use [Portainer](https://www.portainer.io/) for web based Docker management. Portainer is a Docker container by itself and is dead-simple to install and makes life easy.

![Portainer Screenshot for qrapotor Applications](@TODO)

## Exploratory Data Analysis ##

For the exploratory data analysis we use RMarkdown a lot. As the data files are very big we apply some data reducing before. We seperate the core logic and visualisations from the Rmarkdown code to reuse it in other notebooks and in Shiny Apps. We use Shiny Dashboards for on the fly data analysis or for Dashboarding for our clients.

## Data Preprocessing ##

### Data Reducing ###

### Data Cleaning ###


## Training Environment ##

### Trainer Framework ###

### Session Tool ###

## Training Result Analysis ##

### Tensorflow ###

