# Intro to APIs & AJAX

## Learning Objectives

- Describe what an API is and why we might use one.
- Understand the format of API data.
- Explain the common role of JSON on the web.
- Use `fetch` to make GET requests for data.
- Describe what a promise is and how to resolve the promise returned from `fetch`
- Understand the concept of rendering new HTML content using data loaded from an AJAX request.


## SUBMISSION:
There is a required submission for the homework tonight.  You must create an issue on the homework repo with 3 questions you have about any of the topics you have on this reading!

You can use this template for the submission:
```
comfort:
completion:
question 1:
question 2:
question 3:
```


An issue on the repo is **required** and used to grade this assignment. These questions can be about anything in this reading, concepts, confusing sentences from this document, syntax you don't understand, etc.

Without further ado, let's read!

## Intro To API's

**What is an API?**

> Basically, an API is a service that provides raw data for public use.

API stands for "Application Program Interface" and _technically_ applies to all of software design. And _technically_ an API just refers to the interface of any application. This technical definition is rarely used though. 

The term now commonly refers to *web URLs that can be accessed for raw data.*  For API's, the URL's are often called "endpoints".  

**Or in other words an API is just a website that has no view, it only displays data**.  

wait what is an API?

#### an API is just a website that has no view, it only displays data

what is an API??

### an API is just a website that has no view, it only displays data

what is an API???
## an API is just a website that has no view, it only displays data

what is an API????


# an API is just a website that has no view, it only displays data



Ok I hope you understand that an API is just a website that has no view, it only displays data.


Want to see what that is?  Check out this very simple API:  http://api.open-notify.org/astros.json

This is just a website with a url (or "endpoint"), but when you go there you get data, no html, no css, no nothing.  Just pure data. This set of data is from a simple API that shows an up to date list of what astronauts are in space right now.

APIs publish data for public use. As third-party software developers, we can access an organization's API and use their data within our own applications.

> ## Why do we care?

  > Why recreate data when we don't have to? Think about past projects or ideas that would be easier if you could pull in data already gathered elsewhere.

  > APIs can provide us with data that would we would otherwise not be able to create ourselves.  In the case of people in space, you would have no way of creating that data yourself (unless you are a NASA employee or something).  Or in more complex cases, as you'll see later, we can access live and current data for things like weather without having to type that data ourselves!



As we move into building single page applications, now is the perfect time to start understanding how to obtain data on the client side and then render it on the browser.

## Other super simple API examples:

**Check out these stock quotes...**

* [http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL](http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL)
* [http://dev.markitondemand.com/Api/Quote/json?symbol=GOOGL](http://dev.markitondemand.com/Api/Quote/json?symbol=GOOGL)


## Where Do We Find APIs?

APIs are published everywhere. Chances are good that most major content sources you follow online publish their data in some type of serialized format. Heck, [even Marvel publishes an API](http://developer.marvel.com/documentation/getting_started). Look around for a "Developers" section on major websites.

#### List Of Commonly Used API's

Here is a short list of commonly used API's for testing purposes.

| API | Sample URL |
|-----|------------|
| **[Giphy](https://github.com/Giphy/GiphyAPI)** | http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC |
| **[Stocks](http://dev.markitondemand.com/MODApis/)** | http://dev.markitondemand.com/Api/Quote/xml?symbol=AAPL |
| **[Swapi](https://swapi.co/api/people/1/)** | https://swapi.co/api/people/1/ |### Possible API's

### Documentation for Other Good Ones:  
#### Music
- https://bandcamp.com/developer
- https://developers.soundcloud.com/docs/api/reference

#### Gov
- https://developer.cityofnewyork.us/api/open311-inquiry

#### Games
- https://igdb.github.io/api/

#### Social Media
- https://developer.twitter.com/en/docs/tweets/search/overview


## What Is An API Key?

While the majority of APIs are free to use, many of them require an API "key" that identifies the developer requesting data access. Keys are a special password that the API companies use to identify which user is making the request. This is done to regulate usage and prevent abuse. Some APIs also rate-limit developers, meaning they have caps on the free data allowed during a given time period.  The API developers will use the api key to identify which users should be charged.

The Giphy API requires a key.  Open up a browser and see what happens when you try to access its data with a key and without a key:


* No key: [http://api.giphy.com/v1/gifs/search?q=funny+cat](http://api.giphy.com/v1/gifs/search?q=funny+cat)

* With key: [http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC](http://api.giphy.com/v1/gifs/search?q=funny+cat&api_key=dc6zaTOxFJmzC)

**It is very important that you not push your API keys to a public Github repo.**

Especially when the keys are tied to an API with a credit card.  There are melicious bots that scan through github repos looking for people who accidentally posted their keys, then they will sell those keys or use them for themselves!  It's a truly horrible way to lose a lot of money fast.  

> This is especially true when working with [Amazon Web Services (AWS)](https://aws.amazon.com/). Here's an example of a [stolen key horror story](https://wptavern.com/ryan-hellyers-aws-nightmare-leaked-access-keys-result-in-a-6000-bill-overnight).



### Why Just Data?

Sometimes thats's all we need. All this information, from all these browsers and all these servers, has to travel through the network. All of the visual stuff like images, colors, fonts, paragraphs... all of that is almost certainly the slowest part of the request cycle. We want to minimize the bits. There are times when we just need the data. For those times, we want to send data without the fluff.

### What is Structured vs Serialized Data?

All data sent via HTTP are strings. Unfortunately, what we really want to pass between web applications is **structured data** (i.e., arrays and objects). Like the objects we worked with today in transformers are true javascript arrays/objects, which lets us do cool things like loop and filter through it. The data we get from an API is a big big string that only **looks** like a javascript object.  Don't understand what I mean?  This is what it looks like when you go to the giphy API with the JSONview extension off:

![](https://i.imgur.com/0EDJZir.jpg)

It's just a biiiiiiiiiiig string.  So what we do is when we access this biiiiiig string, we need to change that string into a proper javascript object with a datatype of a javascript object.  The act of changing data from a one datatype to another datatype is called "parsing". In the case of API's we need to **parse** the data from a string and into a javascript object.

When we turn a javscript object back into a string, we say that the string is a **serialized** version of the data.  Serialized data has all the same data as a true javascript object but it isn't javacript, it's just a normal old string.  That means that it can also be read by servers that aren't made in javascript.  Python, Java, PHP, .Net, whatever! They can all read serialized data just as well.  This makes serialized data ideal for API's.  And that is why API's send data as a string.

To summarize:
-API's send serialized data (a big big string of data)
-we fetch this big string and parse it into a format we can use (for us, we parse it into a javascript object).

There are **two** major serialized data formats...

#### JSON

**JSON** stands for "JavaScript Object Notation" and has become a universal standard for serializing native data structures for transmission. It is light-weight, easy to read and quick to parse.

```json
{
  "users": [
    {"name": "Bob", "id": 23},
    {"name": "Tim", "id": 72}
  ]
}
```
> Remember, JSON is a serialized format. While it may look like an object, it needs to be parsed so we can interact with it as a true Javascript object.

#### XML

**XML** stands for "eXtensible Markup Language" and is the granddaddy of serialized data formats (itself based on HTML). XML is fat, ugly and cumbersome to parse. It remains a major format, however, due to its legacy usage across the web. You'll probably always favor using a JSON API, if available.  

I've come across a few scenarios in my professional life where the API's will only serve XML.  Rest assured that if NEEDED there are libraries that can covert XML to JSON for you, but doing authentic JSON API endpoints is still preferable.

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

### Parsing JSON

You've seen a few examples of JSON and how data can be organized.  Here is the stock quote for Apple that you saw earlier.  If this data was stored in an object how would you return the `Low` and `High` values?


```
{
	"Data": {
  	"Status": "SUCCESS",
  	"Name": "Apple Inc",
  	"Symbol": "AAPL",
  	"LastPrice": 157.86,
  	"Change": -3.08999999999998,
  	"ChangePercent": -1.91985088536811,
  	"Timestamp": "Thu Aug 17 00:00:00 UTC-04:00 2017",
  	"MarketCap": 815382892080,
  	"Volume": 27940565,
  	"ChangeYTD": 115.82,
  	"ChangePercentYTD": 36.2977033327577,
  	"High": 160.71,
  	"Low": 157.84,
  	"Open": 160.52
	}
}
```


### Working Locally With JSON

JSON is the standard format to orgranize data for servers to send and receive data.  It's so popular that JS has two methods to package it for sending and receiving:

- JSON.stringify() - this will convert something from JSON format into a string.
- JSON.parse() - this will convert a string into a javascript arrays or objects (if possible).


Open up a new terminal window.  Type `node` to open up the javascript console.  Now type the following to try **JSON.stringify()** in the console:

```
const arr1 = [1,2,3];

JSON.stringify(arr1)
```


now do this to try **JSON.parse()** in the console:
```
const dog = `{"name": "fido", "age": 12}`;

JSON.parse(dog);
```

Look at your results.  You can see that for `JSON.stringify` it took an object and returned a string that looks just like the object.

Then `JSON.parse` took a string that looked like an object and returned the actual object.

## AJAX

**So we know what an API is. Now what?**

How can we use an API to dynamically manipulate the DOM with the given data? **AJAX**. As you'll come to learn, this allows us to build single page applications that do not require refreshes.

**AJAX**, which stands for "Asynchronous Javascript and XML," is the method through which we are able to make HTTP requests. The standard requests we will be making are `GET` `POST` `PUT` `PATCH` and `DELETE`.

| Type of Request | What's It Do? | An Example URL |
|-----------------|---------------|----------------|
| `GET`  | Read | [http://swapi.co/api/planets/](http://swapi.co/api/planets/) |
| `POST` | Create | [http://swapi.co/api/planets/](http://swapi.co/api/planets/) |
| `PUT` | Update | [http://swapi.co/api/planets/2/](http://swapi.co/api/planets/2) |
| `PATCH` | Update | [http://swapi.co/api/planets/2/](http://swapi.co/api/planets/2) |
| `DELETE` | Delete | [http://swapi.co/api/planets/2/](http://swapi.co/api/planets/2) |


<details>
  <summary><strong>Q: Why do you think there is a "2" at the end of the URLs in the last three rows? (Hint: you could replace it with any number)</strong></summary>

  > You'll notice that the URLs for `PUT` `PATCH` and `DELETE` end with a number. That's because these actions are updating or deleting a *particular* student. That number is a unique identifier for a particular student defined on the back-end. More on this next week...

</details>

These routes and request types are basically the same as what we did in express, right?

Cool.  Moving on!


## Refresher - Asynchronous vs Synchronous

What is a synchronous operation?

- Sync: A synchronous operation runs immediately after the previous operation and the subsequent operation is not run until after the previous operation completes. This is no problem if the operation is immediate. However, any operations that take significant time to complete will block the next operations. Because JavaScript is single threaded (can only do one thing at a time), blocking operations jam up the entire application making for an intolerable user experience.

What is an asynchronous operation?

- Async: JavaScript environments (i.e. Node.js and browsers) provide built in utilities for doing work that takes time without blocking subsequent calls. What permits this is the `.then` method. The `.then` let's us put a function in memory that won't be called until the long-running operation completes.  

> Note: 
> We used promises and async operations with with pg-promise when handling the SQL queries.  We wrote code that basically said, "hey it might take some time for querying the database to complete, but when it does, run this function which ties that data to `res.local`".


### Promises - Let's go Deeper!

Let's reflect on our experiences with pg-promise to access data from our database.

We send some SQL to our database - what are the possible outcomes of this request?

1. If our SQL is good then it works! we get the data we requested, attach that data to res.locals via `.then`
2. There was a bug in our SQL! send an error to the console via `.catch`.

And before one of these two things happen, it hasn't happened and so we don't know which it is going to be.

All asynchronous operations can be described similarly. Either something is:

1. **pending** and we are anticipating the completion of the asynchronous operation
2. **fulfilled** and the asynchronous operation successfully did what we asked of it
3. **rejected** and the asynchronous operation ran into a problem and wasn't able to complete its task

A promise is an object that represents the **state** of an asynchronous operation.

Again - when a promise gets the `fulfilled` state, the `.then` function will run.  When a promise gets the `rejected` state, the `.catch` function will run.  



#### Fetch

Fetch was added to javascript ES6 so that we can communicate to API's!  It has a pretty funky syntax, but bear with me.  Here is what it looks like in it's simplest form:

```js
fetch('http://api.giphy.com/v1/gifs/search?q=smooth&api_key=dc6zaTOxFJmzC')
   .then((response) => {
     return response.json()
   })
   .then((data) => {
     console.log(data)
     return data
   })
```

The first thing it takes is a string that is the url of the API endpoint we want to use.  In the example above we're using the giphy endpoint from earlier where we want to get the data.

One of the more confusing things about `fetch` is the fact that we have two promises here.  The first `.then` accepts the data coming from the API and converts it to json.  Then when it is done being converted to json the second `.then` method runs and we have `data`!  the variable `data` is the end goal - it is data from the API that has been converted into a javascript object.  Now we have a regular old javascript object that we can do things with!

Like what if we had some function called `renderData` that had a bunch of jQuery stuff which rendered the data from the API call?  We could use it in the second `.then` like:

```js
const renderData = (apiData) => {
  apiData.forEach((el) => {
    let $newDiv = $('<div/>');
    //etc etc
    $('.container').append($newDiv);
  })
}

fetch(url)
   .then((response) => {
      return response.json()
   })
   .then((data) => {
      renderData(data);
   })
```

FEEL THE POWER OF API'S

yesssss


##### When Things Go Wrong

Fetch returns a promise, so just like all other promises we can (and should) use the `.catch` method for when something goes wrong.

```js
fetch('http://api.giphy.com/v1/gifs/search?q=smooth&api_key=dc6zaTOxFJmzC')
   .then((response) => {
     return response.json()
   })
   .then(data) => {
     console.log(data)
     return data
   })
   .catch((err) => {
     console.log(err)
   })
```

### You did it!  

I hope you enjoyed this reading and that this readme is a useful resource for you!  Please don't forget to submit three questions you had from this reading for the homework submission.
