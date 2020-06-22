---
title: "How you avoid being blocked while scraping"
subtitle: "Different approaches that help you with few changes in code"
image: "https://res.cloudinary.com/jenslaufer/image/upload/c_scale,w_800/v1592394603/anita-jankovic-KGbX1f3Uxtg-unsplash.jpg"
image_caption: "Photo by Anita Jankovic on Unsplash"
tags: proxies, proxy, scraping
categories: scraping
layout: post
language: en
---

The last hours I had written a scraper to pull the products of an internet marketplace for a niche analysis. The scraper required some effort because the website uses JavaScript for rendering, so a lot of code is executed in the browser. This circumstance increases the effort needed for scraping. The use of Selenium with a headless chrome browser finally brought success. Now the moment of truth came, I wanted to pull raw data on a big scale for the first time. Full of joyful anticipation I started my scraper, which was supposed to pump the pages into my MongoDB database. Soon I would be able to analyze data, scraping is always the annoying part on the way there. Once you have the raw data, that's half the battle, extracting the interesting fields is the easier part. Because of the parallel processing, the whole thing should be quite fast. While I waited anxiously it happened: The size of the files was suddenly always the same and much smaller than at the beginning and...

__Http Status code: 429__

I realized what had happened: I had been blocked. Of course, I had been aware of this danger from the beginning; however, I had been too impatient. While blocks are often temporary, they prevent you from dragging the amount of data you want. Especially with parallel processing, the danger is severe. You should also be aware that you can be blocked for life if you commit repeated violations.

You are now asking yourself what exactly must be done to avoid blocking. The answer is simple: __You have to use as many different IP addresses as possible from different, worldwide distributed subnets__. In an ideal world, I would make every access from a different IP address. This is difficult to realize or would involve a great deal of effort. In the following, I would like to introduce you to different approaches:

### The naive approach

A first idea is to use freely available proxies, which you probably already use to hide your IP address. I'm sorry to disappoint you because this won't get you anywhere. Many of these proxies are no longer available after some time, or they are extremely slow. Besides, you're not the only one in the world with this idea. Most of the time, you won't get your requests through these proxies.

### The "shoot sparrows with cannons" approach

When I first faced the problem, I had the idea of setting up my own proxy farm using cloud services such as AWS, Azure or GCloud. However, when I did the calculations, I soon realized that this could only be done at very high costs. Let's assume you want to pull data from a website in 20 threads in parallel and randomly rotate 10 IP addresses for each thread, then you need 200 IP addresses and corresponding server resources. This is cost-intensive, and must also include the effort required to maintain this infrastructure. On AWS, an IP address costs about 4 EUR per month. Together with the server, you will make 800-1000 EUR, plus the effort for care and maintenance of your system.

### Use Commercial Proxylists

Another possibility is the purchase of commercial proxy lists. You pay a monthly fee and get access to several proxies that you use for scraping. This works very well, but you have to change your code. The effort is limited, but I don't like the fact that I have to keep the proxy lists somewhere. If I change my contract because I need more proxies, then I have to update my local list. Sure, you can store this in a separate library, but it is a little bit unattractive. For a long time, I have used [Bonanza Proxies](https://proxybonanza.com/?aff_id=831) and have stored the proxy list in a Python module under [scrpproxies on Github](https://github.com/jenslaufer/scrpproxies). The proxies of Bonanza work very well, but I never really liked the library. If I am honest, I am not interested in the exact IP addresses, and I don't want to maintain code for something like that. 

Here is a small code example to send a request with a proxy. I initialize the proxy service first. With getting the module randomly selects a proxy address, which I then use for the request.


```python
from scrpproxies import proxy
import requests

# Initialization of proxy service with credentials
proxies = proxy.BonanzaProxy(username, password)

# Get returns random proxy ip address
proxy = proxies.get()['http']

# use proxy for the request
r = requests.get(url, proxies={"http": proxy})
```


### Proxies via API call

A bit more elegant are APIs that return a proxy address. The beautiful thing is that you don't have to keep local proxy lists. You submit a request and get IP addresses which you then use. [Luminati](https://luminati.io/?affiliate=ref_5ee711e0c7669177ab29ff24) is one such service, but I have only used it for a short time.

This is much more elegant than the previous solution but has a disadvantage. You submit a request to obtain IP addresses, which you then use for the actual request. So I make two requests instead of one. Code-wise this requires a little bit of boilerplate code, but it is manageable.


```python
import requests

# Api call to get Proxy
proxy = requests.get(f"https://luminati.io/api/get_proxy/key={api_key}").text

# use proxy for the request
r = requests.get(url, proxies={"http": proxy})

```

### Black box calling

A much better approach is if you don't even need to act on the proxy used in your logic, because you pass the target URL to the API provider and the API provider processes the call. If the provider is still able to render pages with JavaScript code using a headless browser, you can concentrate on extracting the data. 
You're saving an enormous amount of time. For a long time, I have been looking for such an API. Finally, I found what I was looking for:  [ScraperAPI](https://www.scraperapi.com?fpr=jens78) offers exactly the features I have always been looking for.


```python
import requests

# Scraper Api url template
SCRAPER_API_URL = "http://api.scraperapi.com?api_key={api_key}&url={url}"

# target url
url = "https://somesitetoscrape.com"

# parsed request url
scraper_url = SCRAPER_API_URL.format(api_key=api_key, url=url)

# fetch target url
r = requests.get(scraper_url)

```