# All-Things-Postman

![Postman Logo](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/_README/Postman_Logo.PNG)  

## Why create this repository?

I've been using [Postman](https://getpostman.com) for a while now and I'm still uncovering features that I didn't even know existed - There are a number of How-To guides out there explaining how to use the tool's feature set but as the platform is constantly evolving these can become out of date, really quickly.

My goal is to create a space where I can show examples of some of the many features in the tool and for it to also be a living piece of documentation, that will change over time to reflect the new changes being released.

You can keep up to date with these changes via the [Change Log](https://www.getpostman.com/apps#changelog) and by subscribing to the Postman [Blog](http://blog.getpostman.com/).

One of the main reasons why I'm a fan of the tool is the supporting [documentation](https://www.getpostman.com/docs/) that has been created - Although I will be covering some of the same features I will be including usable content that you can import into your own instance and work through the example at a pace that suits you.

---

Before I start explaining the many different amazing and wonderful features of Postman, I just wanted to share a couple of links to some alternative REST clients - Just to prove that I'm not completely bias towards Postman :)

## [Insomnia](https://insomnia.rest/)
A similar tool to Postman and packed full of cool features. The ability to filter the response data is awesome! [Alan Richardson](https://twitter.com/eviltester) Created a great video explaining some of the features of the tool [3 Reasons to use Insomnia REST Client in your Exploratory API Testing](https://youtu.be/ErDCN_oU9a8).

## [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
[VSCode](https://code.visualstudio.com/) extension giving you the ability to make API requests from inside the editor. Really cool!

These are both excellent REST clients and well worth checking out - Ultimately, It's all down to personal preference and It's completely up to you, to find what meets your own requirement, in your given context.

---

## Let's get started!

I've made a huge assumption that if you're reading this you will probably have Postman downloaded and installed on your machine already - If you don't that is not a problem, just head over to the [Postman](https://www.getpostman.com/) site and grab the flavour that suits your OS. Once you're done - Be sure to come back again. :)

The tool on it's own is quite useless, in order for us to start having some fun and making requests, we need an API with some endpoints that will return some data - This is where a wonderful resource provided by [Mark Winteringham](https://twitter.com/2bittester) comes in handy! Mark has created [Restful-Booker](https://restful-booker.herokuapp.com/), a safe place for people to learn more about API testing and a active platform to try out tools like Postman or any of the other REST clients mentioned.

> "Restful-booker is a Create Read Update Delete (CRUD) Web API that comes with authentication features and loaded with a bunch of bugs for you to explore"

I would recommend taking a look through the API [documentation](https://restful-booker.herokuapp.com/) to get a feel for the type of requests we will be making within the example guides. We'll get very familiar using the GET, POST, PUT and DELETE verbs and pairing these with the powerful features of Postman.

All the requests that we will be making, will be included in a collection, that can be imported directly into your local Postman application. It will start quite basic but we will be building these collections and incorporating features such as:

- Environment and Global Variables
- Preset Headers (Saving you a bunch of time)
- Pre-Request Scripts (Writing basic JavaScript to create new data with each request and other cool things)
- Tests ([Chai](http://chaijs.com/api/) style assertions)
- In-Built Test Runner
- Newman command line collection runner
- Postman Console
- Many many more...

The format will take the form of a series of individual pages where I will explain in a bit more detail what I'm actually doing in Postman when making the requests and this will be backed up by images, gifs, code snippets etc. to try and make the information come to life. I'm a Tester so you can expect me to make observations along the way and I will be noting these down but the main focus of the examples we always be, using Postman and it's features to request data from the Restful-Booker API.

It will be an ever evolving space so if you would like me to add details covering certain features or to expand the examples in the repository - Please give me a shout [@dannydainton](https://twitter.com/DannyDainton) I'm always available to chat!

---

## Example Guides

All requests in the examples will be made to the Restful-Booker API - You can find the available endpoints [here](https://restful-booker.herokuapp.com/).

### **Current Postman version being used:** _6.0.10 Windows x64_

### 01 `GET` /ping

In this example we will send a request using the `/ping` route to ensure that the API is active and able to receive requests.

- [Ping the API](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/01_pingTheAPI.md)

### 02 Create an Environment file

In this example we will be creating an Environment file and using data from this file within our requests.

- [Create environment file](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/02_createEnvironmentFile.md)

### 03 `GET` /booking

For this example we will be making requests to the `/booking` route and using some different parameters in the requests to filter the response data.

- [Get all bookings](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/03_getAllBookings.md)

### 04 Preset Headers

For this example we will be looking at the Preset Headers feature.

- [Pre Set Headers](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/04_preSetHeaders.md)

### 05 Importing Files

In this example we will look at some of the methods of importing various different files into Postman.

- [Importing Files](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/05_importingFiles.md)

### 06 `GET` /booking/{id}

In this example we will be getting the data for a single booking from the API. We will explore the endpoint using `Request Headers` and also taking a basic look at the `Pre-request Scripts` feature to make our request a little bit more dynamic.

- [Get a single booking](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/06_getSingleBooking.md)

### 07 Creating our first test

In this example we will be taking our first look at the `Tests` feature. We will create a basic test to assert against some response data returned from the Restful-Booker API.

- [Creating our first test](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/07_creatingOurFirstTest.md)

### 08 Extending our tests

For this example we will be taking the knowledge gained from creating our first basic test and extending this to cover more of the response data.

- [Extending our tests](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/08_extendingOurTests.md)

### 09 Global and Dynamic variables

In this example we'll be looking at how to Create, Use and Clear Global variables. We will also be taking a look at the Dynamic Variables that Postman offers.

- [Global and Dynamic variables](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/09_globalAndDynamicVariables.md)

### 10 `POST` /booking

In this example we'll be using the `POST` method to send data to the `/booking` endpoint and create some new bookings based on the information we provide in the request.

- [Create new bookings](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/10_createNewBookings.md)

### 11 Dynamically create new bookings

For this example we will be using Postman to dynamically generate data using some of the built-in modules to create new bookings.

- [Dynamically create new bookings](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/11_dynamicallyCreateNewBookings.md)

### 12 Introduction to the Collection Runner

In this example we will be looking a bit closer at the `Collection Runner` which allows us to run a series of requests.

- [Introduction to the Collection Runner](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/12_introductionToTheCollectionRunner.md)

---
### More examples to follow shortly...
