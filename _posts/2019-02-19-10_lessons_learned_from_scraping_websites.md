---
layout: post
title: "10 Lessons Learned from Scraping Websites"
subtitle: "Valuable insights which I gained from retrieving data from many websites over the last years which I want to share with you"
categories:
  - "data retrieval"
show_comments: true
tags: scraping, "data retrieval"
image: '/assets/img/scraping.jpg'
language: en
output:
  html_document: default
---



> "Data is the new oil. It's valuable, but if unrefined it cannot really be used. It has to be changed into gas, plastic, chemicals, etc. to create a valuable entity that drives profitable activity; so must data be broken down, analysed for it to have value." - Clive Humby

A frequent criticism of Kaggle and MOOC (Massive Open Online Courses) is that they are reducing data science to predictive modelling and they are using pre-cleaned or non-real-world datasets. Data Science is more than that. Data retrieval and cleansing are essential parts of the field too. You and I know that data can make a difference. If you can get better data, you perform better. Do you think data retrieval is a task for a data engineer? In a small startup or as an entrepreneur following your passions, you don't have access to one. You might have any data in the beginning. What is the solution to this problem? 

Scraping the data from websites is a solution. Web scraping has a bad reputation, as people think that you are stealing the data. It is unethical in my eyes in case you are building up a competitive product with the data. But what is about taking the labelled photos to teach a neural network? I think this is OK. You might give the company even something valuable back or push their business with your data product.

I got my hands dirty over the last years to learn data science and to build up my own data products. In this time I scraped data from many websites and learned a lot. I want to share the most important lessons with you:

### 1. Check the Legal Stuff

Before you scrape a website __ALWAYS__ check it's [Terms of Service](https://en.wikipedia.org/wiki/Terms_of_service) and the  [robots.txt](https://en.wikipedia.org/wiki/Robots_exclusion_standard) to answer the question what you are allowed to fetch and what not. This saves you a lot of trouble and the risk of a court case. Please remember that scraping is a grey zone. Do you want to get sued by Google or Amazon?

### 2. Invest time for technical research

It's worth to invest time for technical research before you start scraping. Sometimes you don't need to scrape/parse the raw HTML files as there exists an internal REST API in the backend. Modern webpages use JavaScript Frameworks which use [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs. You even not able to parse HTML as the logic is executed in the browser. In this scenario, you might need a framework like [Selenium](https://www.seleniumhq.org/), that uses a modern browser in headless mode that is executing the JavaScript code. That are bad news, but you have the chance that there is a RESTful API in this case. These APIs just send JSON or XML back, which is even better than extracting the data from HTML. 

You also need to check what headers your browser sends to your website. The developer tools of the browsers e.g [Mozilla Browser Developer Tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools) help you with this.


### 3. Dockerize your scraping application

Build a docker container for your scraping application. Scraping configurations can be a bit complicated, especially if you fetch the data from the resource with Selenium, which uses a browser like Chromium or Firefox. You need operating system dependent drivers for this situation. You can use my [docker image with Firefox/Geckodriver and Chromium/Chromedriver](https://cloud.docker.com/repository/docker/jenslaufer/docker-chromedriver-geckodriver) on Dockerhub as a base image for your scraping application. The source can be downloaded from [Github](https://github.com/jenslaufer/docker-chromedriver-geckodriver). Use [Docker Compose](https://docs.docker.com/compose/): It is a tool for defining and running multi-container Docker applications. This way you can set up a database with no effort along with your scraping container.
If you are not familiar with [Docker](https://github.com) then try it out. It's a secret weapon for reliable runtime environments for all kind of software projects.

### 4. Always use first class proxies for scraping

Use proxies already during development. Rotate the proxies randomly. This saves you a lot of trouble. My rule of a thumb is to use 10 proxies per scraping process/thread. Try to avoid free proxies lists. The proxies on these lists are often slow or already blocked. It is a waste of time to try them out. Google for proxy providers. You need to invest some money here. You can even buy non-shared proxies, but they have their costs. I can recommend [ScraperAPI](https://www.scraperapi.com?fpr=jens78) and [Bonanza Proxies](https://proxybonanza.com/?aff_id=831), which I use both.


### 5. Write a dump scraper first, then the parser


Write first a scraper that just fetches a resource and saves it. Ensure that this data is the same as the data downloaded from your browser. If this is the case you can increase the number of processes/threads and load more data. Once you have a bunch of raw data files (not too many!) you start to develop the parser. Extract the data you need and check if the data is OK. Do an exploratory data analysis, check for data anomalities like missing values or outliers to ensure a high quality parsing logic. Log all errors while you parse. This way you can find out, if the logic works for all resources the same. 

__Keep the scraping logic seperated from the parsing logic.__


### 6. Scrape every unique URL rarely

I try to scrape each URL only once. Or very very rarely!

You don't want to download the same resource over and over again, especially during development. We all have a kind of trial and error programming style. We write a piece of code and test if it works or not, do some bug fixing. This can be fatal during scraper development as you risk to get blocked already in this phase. The block is sometimes only for a few hours, but the block blocks your programming. Seperating the parsing logic from the scraper logic helps you to reduce the lines of code in which you can make errors. Keep this in mind:

- Don't send too many useless requests to a website. It's also a kind of respect to the valuable data the site offers you. You don't want to get blocked or banned as said before.
- Don't download massive amount of data in the beginning in multiple threads/processes. Try to download single resources until your scraping code works fine.
- Implement a flag that you are able to force the resource download when it is outdated. Outdated means for me several weeks rather than several hours. This depends for sure also what you want to do. If you need the data more often, try to scrape with more proxies.

### 7. Always keep the raw files (raw oil!)

It's essential to keep the raw data once the resource is scraped. This has several advantages:

1. You can improve the parser logic in as many iterations as you need, without having risk of being blocked
2. Raw data are like diamonds. You can find more interesting stuff in the data later, stuff you weren't interested in first iterations
3. You reduce the blocking risk

### 8. Mimic a real browser with your headers

Set proper headers, when fetching a resource from a website. Check what your browser is sending and mimic a real browser in your code. In my experience one of the most important one is the user agent header. I am sharing a very [simple python module for useragent rotation](https://github.com/jenslaufer/pyuseragents) with you. It takes randomly one user agent out of thousands.

### 9. Use a database for your data

Save the raw data in a database. I love [MongoDB's GridFS](https://docs.mongodb.com/manual/core/gridfs/) for this task. The API is very simple. You have different advantages with this:

- You can save multiple metadata with the file (e.g. the query parameters you used to get the resource, retrieval date etc.), which is difficult in your local filesystem.
- You can build a history of raw files for a resource
- You overcome file system limitations like the maximum number of files/directory

Use a database for the parsed data. You are getting an improved data access through the use of query languages. 

[MongoDB](https://www.mongodb.com/) is an excellent choice here, as you don't need a scheme, you can just save the data as it is. Especially if you able to scrape from an internal API. From APIs you get often JSON, and you can toss the JSON as it is into the MongoDB.

### 10. Ensure data quality with a data analysis

Once you have a bunch of raw files and your parser is written do an Exploratory Data Analysis (EDA) to check the data quality. Check for missing values. I wrote a blog post about [Missing values visualization with tidyverse in R](http://jenslaufer.com/data/analysis/visualize_missing_values_with_ggplot.html) which might help you. Do a univariate data analysis to check for anomalities for each feature. Do a multivariate analysis to check, if values in linked features are consistent. Visualizations are your best friends here.

This step is essential and missed out by many people. You find the weak points in your parsing logic. You might go through several iterations to improve the parsing logic until you are happy.

_Feel free to [send me a message](mailto:jenslaufer@gmail.com) in case you need support in your scraping project._
