# Dynamically create new bookings

For this example we will be using Postman to dynamically generate data using some of the built-in modules to create new bookings.

---

In the last [example](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/10_createNewBookings.md), we created new bookings using the `/booking` endpoint. This was based on data manually entered into the request body, we were adding the following sample data taken from the Restful-Booker API documentation.

```json
{
  "firstname" : "Sally",
  "lastname" : "Brown",
  "totalprice" : 111,
  "depositpaid" : true,
  "additionalneeds" : "Breakfast",
  "bookingdates" : {
    "checkin" : "2013-02-23",
    "checkout" : "2014-10-23"
  }
}
```

Using this data set to send a single request each time works perfectly, individual values can be changed to create a brand new booking but what if there was a way to eliminate the manual burden and add the test data dynamically, so that we get a new booking each time we press `Send`...

I'm going to be introducing a couple of new things that you may not be familiar with but I will do my best to explain what they are and why I'm using them - As always there will be lots of example code snippets and images/gifs to bring the information to life.

We'll get straight into it and start creating the data. All the magic happens in the `Pre-Request Script` tab, from this section we will be adding some JavaScript code to create pieces of data. This new data will be assigned to an `environment variable` using the `pm.environment.set('variable_name', 'variable_value')` syntax, once set, we can use these variables in the body of our `POST` request.

Postman comes with several build-in modules that can be used in different ways within areas of the application. We're going to be using the [`Lodash`](https://lodash.com/docs/4.17.4) and [`Moment`](https://momentjs.com/docs/) modules - These are very powerful and popular modules that are used in many different commercial projects. I was very happy to see them being used in Postman. We will be using these modules in partnership with the Postman `pm.*` API functions.

As the `/booking` endpoint takes a specific set of data we need to get all that together to make our request valid. I'm a fan of all the free sites out there that take the pain away from data creation, services like [Mockaroo.com](https://www.mockaroo.com/) and [GenerateData.com](https://www.generatedata.com/) are great but I'm going to be using [Randomuser.me](https://randomuser.me/) for this, mainly because it has a very simple API that we can used to get the data we need.

We can send a `GET` request to the randomuser API using the following URL `https://randomuser.me/api/` - This will return the details of a _randomly_ generated person with a bunch of useful data points.

![randomuser API](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/11_dynamicallyCreateNewBookings/randomuser_API.PNG)

As we only really want _some_ of this response data to populate the `firstname` and `lastname` properties for our `/booking` request, we need to extract that information from the response. A great way to work out the correct reference point is by logging all the response data to the Postman Console using `console.log()`. You can open this by selecting the `Postman Console` icon which can be found on the bottom menu bar of the application.

![Getting data](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/11_dynamicallyCreateNewBookings/Getting_Data.PNG)

In the image above, you can see that I've logged out the whole request body to the console and I'm using this to navigate through the JSON to get to the values I require. I simply think of it as stepping down a level each time, to reach a certain destination. Once I know the location, I can log this to the console to see the value.

```javascript
// Log out the whole response body as JSON
console.log(pm.response.json())

// Moving down through the JSON to the values that we need
// The `results` property is an array and it's the first in the list so we use [0]
// Moving down a level we need the `name` property
// Finally we want the `first` and `last` property values
console.log(pm.response.json().results[0].name.first)
console.log(pm.response.json().results[0].name.last)
```

How does that help with the Restful-Booker API? Good question - I've previously mentioned the set of `pm.*` API functions that Postman has built-in to the application, we can use the `pm.sendRequest()` function in a `Pre-Request Script` to get the random user data, before we even send our _main_ request :)

This is where things start to get a whole lot more interesting - Within the `Snippets` section of the `Pre-Request Script` tab there is an option called `Send a request`, by selecting this it will give you a template that you can use to send an additional request.

![Send A Request](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/11_dynamicallyCreateNewBookings/Send_A_Request.PNG)

```javascript
pm.sendRequest("https://postman-echo.com/get", function (err, response) {
    console.log(response.json());
});
```

This generated template is using one of the Postman test endpoints to send a `GET` request to `https://postman-echo.com/get` and then logging the response out to the Postman Console.

![Pre Request](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/11_dynamicallyCreateNewBookings/Pre_Request.gif)

In the clip you can see that the request to the `https://postman-echo.com/get` is sent just before the _main_ request to the Restful-Booker `/ping` route was made, the response was then logged to the console. We can use this Postman function and point it at the `randomuser` API URL to get the random data. We want to _store_ this data rather then sending it straight to the console so we need to capture this as an `environment variable`. Let's take a peek at what that would look like...

```javascript
pm.sendRequest("https://randomuser.me/api/", (err, res) => {
    // Get the values from the response and store them as a variable
    var firstname = res.json().results[0].name.first
    var lastname  = res.json().results[0].name.last

    // Format and save the values as an environment variable to use in the body of the next request
    pm.environment.set("first_name", JSON.stringify((_.capitalize(firstname))))
    pm.environment.set("last_name", JSON.stringify((_.capitalize(lastname))))
})
```

![Get Names](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/11_dynamicallyCreateNewBookings/Get_Names.gif)

There a couple of additional things happening in the code above, the `firstname` and `lastname` values from the randomuser API are always returned in lowercase and I really want my names to start with a `Capital` letter. Here's the first time we get to use the `Lodash` module. `Lodash` gives us lot's of utility functions that we can use to perform certain actions quicker and with less code, these _could_ be done in native JavaScript but it would start to look a bit messy. I'm using the [`capitalize()`](https://lodash.com/docs/4.17.4#capitalize) function which converts the first character of a _string_ to uppercase. Each `Lodash` module can be easily referenced using the `_` variable before any of the functions - Think of it as doing a `const _ = require('Lodash')` behind the scenes to set the variable. To finish off the code, we need to store the `names` as a string, in order to use them in the `/booking` request so we use the native `JSON.stringify()` function to convert the value to a string. 

There were a few different things happening there so please take the time to be comfortable with what's going on and ensure that you're happy with what the full piece of code is trying to achieve.

We also have a few other property values to populate in our request to create our new booking, we can use a combination of native JavaScript and another module called `Moment` to generate the data we need - This module will be used to simplify the creation of the time values we need, it's a widely used JavaScript library and it's just awesome.

As well as using the `Moment` module we will also be using two more `Lodash` functions:
- [random()](https://lodash.com/docs/4.17.4#random)
- [shuffle()](https://lodash.com/docs/4.17.4#shuffle)

These functions are hopefully self explanatory, the `random()` function takes two numbers (inclusive lower and upper bounds) and returns a value between those given numbers. The `shuffle()` function does exactly that, we create an list of values and each time the request is made, the function will _shuffle_ the order of the list and we set the first value `[0]` as our environment variable. Adding `[_.random(number_of_items_in_the_list)]` instead of `[0]` would do a similar thing, it's up to you to find what works best. :)

```javascript
pm.environment.set("total_price", _.random(0, 1000))

const depositPaid = [true, false]
pm.environment.set("depositPaid", _.shuffle(depositPaid)[0])

const items = ["None", "Breakfast", "Lunch", "Dinner", "Late Checkout", "Newspaper", "Extra Pillow"]
pm.environment.set("additional_needs", JSON.stringify(_.shuffle(items)[0]))
```

Using `Moment` is slightly different - We need to _bring in_ this module to our script and this is done using the `const moment = require('moment')` syntax. For demo purposes I've added this just above the code where I'm using it but _normally_ I will have all my `require` statements at the top of my code - Just a personal preference. For the sample data set we know that the format of the date needs to be 'Year-Month-Day' and we can set this using the `format()` function. I've also used the `add()` function on the `checkout` date to ensure that it's after the `checkin` date.

```javascript
const moment = require('moment')
pm.environment.set("check_in", JSON.stringify(moment().format('YYYY-MM-DD')))
pm.environment.set("check_out", JSON.stringify(moment().add(_.random(1, 14), 'days').format('YYYY-MM-DD')))
```

Pulling all those things together, before the main request is made Postman will create all the data and store them as `environment variables`. You can see in the clip that these are saved against the _Restful_Booker_Environment_ file.

![All Data](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/11_dynamicallyCreateNewBookings/All_Data.gif)

That's a lot of new information to take in and digest, unfortunately, we're not finished yet. To use the saved `environment variables` in our request we need to make references to those in our request body - Thankfully, this is something that we have covered before. :) The request body should look like this once you've entered the details.

![Request Body](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/11_dynamicallyCreateNewBookings/Request_Body.PNG)

### Finally...

We may have a need to clear out all the saved data in the `environment variables`, from the previous request. This JavaScript function _can_ be added to the `Tests` tab that will do just that...

```javascript
function cleanup() {
    const clean = ['first_name', 'last_name', 'total_price', 'depositPaid', 'check_in', 'check_out', 'additional_needs']
    for(let i = 0; i < clean.length; ++i){
        pm.environment.unset(clean[i])
    }
}
cleanup()
```

This might not be the most elegant or efficient code to clear out these variables but it's certainly a starting point and this can be refactored at any time. Basically, I have an array of all the `environment variables` that I've used for the request and I've assigned these to the `clean` variable. The `for` loop will then iterate through all the items in the array and pass each value to the `pm.environment.unset(clean[i])` function. This will then clear each of the `environment variables` one by one, giving you a `Tear Down` process.

Let's see all of this in action and create some new bookings each time we hit the `Send` button...

![Create Bookings](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/11_dynamicallyCreateNewBookings/Create_Bookings.gif)

Well done - You made it to the end of the example, lot's of new techniques have been used here to create the data and hopefully I've explained them enough to demonstrate what's going on in each of the different areas. The _Restful_Booker_Collection_ has been updated with the new requests using both `JSON` and `XML`, feel free to either attempt the example yourself or import the new collection to get the requests with all the information. If you get stuck along the way, please get in touch with me on Twitter @dannydainton.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
