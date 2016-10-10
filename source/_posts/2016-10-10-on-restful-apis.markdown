---
layout: post
title: "On RESTful APIs"
date: 2016-10-10 04:10:00 -0400
comments: true
categories: 
- REST
- API
- Demo
---
<strong>What is an API?</strong>

API stands for Application Programming Interface.

It is an easy way for interacting with another application. For example, I could make a GET request to a website’s API and it would give me back data as a response. There are also other requests you can make to an API that are covered further down. The API is acting as the middle-man between the programmer and the application. The providers of the application’s service would give instructions on how to properly format the requests I make to the API and how I can interact with it. It’s a contract where the API will accept my request, and if it is allowed, will then process it. Having an API gives third-party developers an easy way to use a website’s services. Thus, we can use an existing web application for our own purposes.

For example, let’s say I want to make a web application that has a lot of pictures of cute animals. Where am I going to find all of these cute animals? While I could manually search for them and save them for my website, that seems like a hassle. Instead, I could make use of Reddit’s API to get these cute pictures. Reddit has a subreddit called /aww where users upload cute pictures of animals. I can make a GET request to Reddit’s API, and in return it could give me back a response (in this case, JSON) that contains information from the /aww subreddit including the images that users have been posting. 

In this case:

GET <a href ="https://www.reddit.com/r/aww.json">https://www.reddit.com/r/aww.json</a>

If I make a GET request to this URL, I will get JSON back and from the response I get, I can also get all sorts of information to use on my cute animal website such as links to the pictures. This is easier than scraping the data directly from reddit.

Other examples of using an API could be to get back weather data. OpenWeatherMap has an API that let’s you easily get back information on the weather. To know which requests give back the responses you want, you can view their <a href="https://openweathermap.org/api">API docs</a>. In this case, you would need an API key to make requests. The key would help OpenWeatherMap control how their API is being used. One example could be limiting the number of requests you can make to the API within a certain amount of time.

<strong>What is REST?</strong>

REST stands for Representational State Transfer. 

It is an architectural style that helps with communications between computer systems. It’s a stateless communications protocol as in the state of the client session is not stored server side and HTTP requests happen in isolation. This means that it’s only concern is when a request is made and it does not care for past requests. There are benefits of having a stateless transaction. For one thing, it reduces memory storage and it’s also easier to cache. As the client is not aware of the implementation, it’s also more scalable.

Generally, a RESTful service would be based on HTTP. 

<strong>RESTful API?</strong>

Now, all of the previous examples of API calls are all known as RESTful APIs. Most RESTful APIs use HTTP requests, which are stateless. In the previous examples, I only showed one HTTP request method: GET.

However, there are more and they all do different things.

GET method requests data.

POST method submits data.

PATCH updates data.

DELETE deletes data.

There are more methods, but those are the main ones used. It should also be noted that not all HTTP methods are considered safe. Safe methods are those that do not modify data unlike methods such as - POST, PATCH, DELETE. It isn’t super common to allow unsafe methods because there could be repercussions when manipulating the resource especially if multiple people are doing so at the same time.  

<strong>Example API</strong>

I made a simple demo API, you can see it <a href ="https://simpledemoapi.herokuapp.com/">HERE</a> and play around with it.





