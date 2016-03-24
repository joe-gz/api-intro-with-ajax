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

### Think, Pair, Share (5 mins)

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

**It is very important that you not push your API keys to a public Github repo.**

## AJAX

**So we know what an API is, now what?**

How can we use an API to dynamically manipulate the DOM with the given data? In Rails, we would have to make a request through the server, and refresh the page with returned data.  Now we can do this on the client side! As you'll come to learn, this allows us to build single page applications that do not require refreshes.

How do we do this?
*AJAX* (Asynchronous Javascript and XML) is the method through which we are able to make the usual HTTP requests: 'GET' 'POST' 'PUT' 'PATCH' 'DELETE' (depending on the access we have) to a given API.

## GET data from an external API using AJAX

### I Do: GET example

Let's build a very simple app that posts a movie title and poster after searching for it!
[OMDB movie search](https://github.com/joe-gz/omdb-api)

The starter code is linked above. It contains a basic HTML/CSS/JS setup. If you open up the HTML in the browser, you will notice that searching for something returns no results.

Let's go ahead and add in the AJAX request:

```js
// get value from search input field
var keyword = $("input[name='keyword']").val();
var url = "https://www.omdbapi.com/?t="+keyword
$.ajax({
  url: url,
  type: "GET",
  dataType: "json"
}).done ( function(response){
  console.log(response);
  // call movie function below to append movie titles
  movie(response);
}).fail ( function (){
  console.log("fail");
}).always( function(){
  console.log("Something happens");
})
```

**What does all of this mean?**

$.ajax takes an object as an argument with at least three key-value pairs...
(1) The URL endpoint for the JSON object.
(2) Type of HTTP request.
(3) Datatype. Usually JSON.

The `url` that we've included is the api url from OMDB, with the added keyword that we are searching for. The `type` is simply the verb we want performed; in this case GET. And, the `datatype` will typically be `json`.

### Promises

Following `datatype`, there are three methods that we call promises.  In short, these tell the `$.ajax` request what to do if the request is successful, fails, or both.

* `.done` requires a callback that determines what we do after a successful AJAX call
* `.fail` requires a callback that determines what we do after an unsuccessful AJAX call
* `.always` requires a callback that determines what we do regardless of a successful or unsuccessful call

*Bonus*: you'll notice that if you simply search for "Star Wars", it doesn't really work! why is that? Is there another url you can use from OMDB API that would return all Star Wars movies?

### You Do: GET from Tunr

Fork and clone another [Tunr](https://github.com/ga-wdi-exercises/tunr_rails_ajax) repo!
Once you've cloned the repo, `cd` into it and run the usual commands...

```bash
$ bundle install
$ rake db:create db:migrate db:seed
```

We can now use `$.ajax()` to make asynchronous HTTP requests to our Tunr app! Let's go ahead and create a new Artists controller action and corresponding view: `test_ajax`

#### Setting up a view to test AJAX with (10/85)
Let's update our routes in `config/routes.rb` for a new route to test all of our AJAX calls in:

```ruby
get '/test_ajax', to: 'artists#test_ajax'
```

in `app/controllers/artists_controller.rb`:

```ruby
# We're just creating this so we can test AJAX on a separate view, test_ajax.html.erb.
def test_ajax
end
```

Create `app/views/artists/test_ajax.html.erb` and place the following content:

```html
<div class="test_ajax_get">AJAX GET!</div>
<div class="test_ajax_post">AJAX POST!</div>
<div class="test_ajax_put">AJAX PUT!</div>
<div class="test_ajax_delete">AJAX DELETE!</div>
```

#### AJAX GET (5/90)
Great, everything's working. Let's try doing a `GET` request to our API like we did with the Weather underground API. In `app/assets/javascripts/application.js`:

```js
$(document).ready(function(){
  $(".test_ajax_get").on("click", function(){
    $.ajax({
      type: 'GET',
      dataType: 'json',
      url: "http://localhost:3000/artists"
    }).done(function(response) {
      console.log(response);
    }).fail(function(response){
      console.log("Ajax get request failed.");
    })
  })
})
```

> If we access the response object, we can see all of the artists that were seeded in the database. Inside the done promise, we can interact with and display all the contents of the response.

*Bonus*: Render the data on the browser!
*Hint*: similar to the movie app, check the response

## AJAX and the rest of CRUD

### I Do: POST a new artist

### You Do: Finish Tunr Artist CRUD!

### Bonus You Do: CRUD for Songs

## Conclusion (5 minutes / 2:40)

Review Learning Objectives

## Hungry for More?

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

## Resources:
* [Andy's blog](http://andrewsunglaekim.github.io/Server-side-api-calls-wrapped-in-ruby-classes/)
* [Postman](https://www.getpostman.com/)
* [Intro to APIs](https://zapier.com/learn/apis/chapter-1-introduction-to-apis/)
* [Practice with APIs](https://github.com/ga-dc/weather_teller)
