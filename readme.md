# APIs

## Learning Objectives

- Describe what an API is, and why we might use one.
- Explain the common role of JSON on the web.
- Use jQuery $.ajax() method to make asynchronous GET requests for data.
- Use jQuery's 'promise-like' methods to handle AJAX responses asynchronously.
- Render new HTML content using data loaded from an Ajax request.
- Perform POST, PUT, and DELETE requests to an API to modify data.


## Framing

**What is an API?**
>Basically, an API is a service that provides raw data for public use.

API stands for "Application Program Interface", and technically applies to all of software design. However, since the explosion of information technology, the term now commonly refers to web URLs that can be accessed for raw data.

APIs publish data for public use. As third-party software developers, we can access an organization's API and use their data within our own applications.

**Why do we care?**

Because why recreate data when we don't have to? Think about past projects or ideas that would be easier if you could pull in data already gathered elsewhere..

## API Exploration

**Check out these stock quotes...**

* [http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL](http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL)
* [http://dev.markitondemand.com/Api/Quote/json?symbol=GOOGL](http://dev.markitondemand.com/Api/Quote/json?symbol=GOOGL)

Form pairs and explore the API links in the below table. Record any observations that come to mind. In particular, think about what you see in the URL and the API response itself.

| API | Sample URL |
|-----|------------|
| **[This for That](http://itsthisforthat.com/)** | http://itsthisforthat.com/api.php?json |
| **[iTunes](https://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)** | http://itunes.apple.com/search?term=adele |
| **[Giphy](https://github.com/Giphy/GiphyAPI)** | http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC |
| **[OMDB API](http://www.omdbapi.com/)** | http://www.omdbapi.com/?t=Game%20of%20Thrones&Season=1 |
| **[StarWars](http://swapi.co/)** | http://swapi.co/api/people/3 |
| **[Stocks](http://dev.markitondemand.com/MODApis/)** | http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL |

### Why Just Data?

Sometimes thats's all we need. All this information, from all these browsers and all these servers, has to travel through the network. That's almost certainly the slowest part of the request cycle. We want to minimize the bits. There are times when we just need the data. For those times, we want a concise format.   

## What is serialized data?

All data sent via HTTP are strings. Unfortunately, what we really want to pass between web applications is *structured data*, as in: native arrays and hashes. Thus, native data structures can be *serialized* into a string representation of the data. This string can be transmitted, and then parsed back into data by another web agent.  

There are **two** major serialized data formats:  

* **JSON** stands for "JavaScript Object Notation", and has become a universal standard for serializing native data structures for transmission. It is light-weight, easy to read, and quick to parse.

```json
{
  "users": [
    {"name": "Bob", "id": 23},
    {"name": "Tim", "id": 72}
  ]
}
```
> Remember, JSON is a serialized format. While it may look like an object, it needs to be parsed so we can interact with it as a true Javascript object.

* **XML** stands for "eXtensible Markup Language", and is the granddaddy of serialized data formats (itself based on HTML). XML is fat, ugly, and cumbersome to parse. However, it remains a major format due to its legacy usage across the web. You'll probably always favor using a JSON API, if available.

```
<users>
  <user id="23">
    <name><![CDATA[Bob]]></name>
  </user>
  <user id="72">
    <name><![CDATA[Tim]]></name>
  </user>
</users>
```

**Many APIs publish data in multiple formats, for example...**

* [http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL](http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL)
* [http://dev.markitondemand.com/Api/Quote/xml?symbol=AAPL](http://dev.markitondemand.com/Api/Quote/xml?symbol=AAPL)

## Where Do We Find APIs?

APIs are published everywhere. Chances are good that most major content sources you follow online publish their data in some type of serialized format. Heck, [even Marvel publishes an API](http://developer.marvel.com/documentation/getting_started). Look around for a "Developers" section on major websites, or ask the Google Answer-Bot.

> Except for ESPN and most major sports entities :(
  - [ESPN Dev](http://espn.go.com/apis/devcenter/)

**That sounds hard. Can't you just give me a freebie?**

Okay... try the [Programmable Web API Directory](http://www.programmableweb.com/apis/directory) or the [Public APIs Directory](http://www.publicapis.com/).

## What Is An API Key?

While the majority of APIs are free to use, many of them require an API "key" that identifies the developer requesting data access. This is done to regulate usage and prevent abuse. Some APIs also rate-limit developers, meaning they have caps on the free data allowed during a given time period.

**Try hitting the Giphy API...**

* No key: [http://api.giphy.com/v1/gifs/search?q=funny+cat](http://api.giphy.com/v1/gifs/search?q=funny+cat)

* With key: [http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC](http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC)

> It is very important that you not push your API keys to a public Github repo. [Figaro](https://github.com/laserlemon/figaro) is a useful gem to utilize environment variables for hiding API keys.

## Good Starter APIs

There is an immense number of APIs out there from which you can pull data.

| API | Sample URL |
|-----|------------|
| **[This for That](http://itsthisforthat.com/)** | http://itsthisforthat.com/api.php?json |
| **[iTunes](https://www.apple.com/itunes/affiliates/resources/documentation/itunes-store-web-service-search-api.html)** | http://itunes.apple.com/search?term=adele |
| **[Giphy](https://github.com/Giphy/GiphyAPI)** | http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC |
| **[OMDB API](http://www.omdbapi.com/)** | http://www.omdbapi.com/?t=Game%20of%20Thrones&Season=1 |
| **[StarWars](http://swapi.co/)** | http://swapi.co/api/people/3 |
| **[Stocks](http://dev.markitondemand.com/MODApis/)** | http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL |
> Note the variety in the URLs used to access these APIs. Do any of them look similar to what you've made in class?

## Rails and JSON

## Conclusion (5 minutes / 2:40)

Review Learning Objectives

## Resources:
* [Andy's blog](http://andrewsunglaekim.github.io/Server-side-api-calls-wrapped-in-ruby-classes/)
* [Postman](https://www.getpostman.com/)
* [Intro to APIs](https://zapier.com/learn/apis/chapter-1-introduction-to-apis/)
* [Practice with APIs](https://github.com/ga-dc/weather_teller)

## More

### Postman: A Closer Look at an API Request

Let's make a basic HTTP request to an API. While we can technically just do this in the browser, we're going to use Postman - a Chrome plug-in for making HTTP requests - so we can look at it in more detail.  
Steps  
  1. [Download Postman](https://www.getpostman.com/).  
  2. Type in the "url" of an API call.  
  3. Ensure the "method" is "GET".  
  4. Press "Send".  

Here's an example of a successful `200 OK` API call...

![Postman screenshot success](http://i.imgur.com/2TADr4J.png)

And here's an example of an unsuccessful `403 Forbidden` API call. Why did it fail?

![Postman screenshot fail](http://i.imgur.com/r3nIhGH.png)
