# `GET` /booking/{id}

In this example we will be getting the data for a single booking from the API. We will explore the endpoint using `Request Headers` and also taking a basic look at the `Pre-request Scripts` feature to make our request a little bit more dynamic.

---

In a previous example we made a basic `GET` request to the `/booking` endpoint and this returned the following response.

![All Bookings Response](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/06_getSingleBooking/All_Bookings_Response.PNG)

So for this example, we will be taking those `bookingid` values seen in the response above and using the `/booking/{id}` endpoint to return data relating to that Id. Along the way we will be using several different Postman features to interact with the endpoint and change the type of data returned using a specific `Request Header`. As this is a new endpoint, let's look at the [documentation](https://restful-booker.herokuapp.com/#get-bookingid) to see what is on offer to us.

![Get Single Booking RBD](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/06_getSingleBooking/Get_Single_Booking_RBD.PNG)

The endpoint takes an `id` parameter value in the request, we know from the `/booking` request that we made, that this is a number but in the documentation it states that this is a `string` value and not an `integer` so I'm instantly thinking of different ways to manipulate that value to expose potential issues.

### Get information about a single booking

We have our list of available id's from the previous request, we're going to use the first `id` in the request to the `/booking/{id}` endpoint and explore the data returned in the response.

In the clip below, I am sending the following request `{{baseURL}}/booking/10` - The `baseURL` value is set in the _Restful_Booker_Environment_ file.

![Get Single Booking](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/06_getSingleBooking/Get_A_Single_Booking.gif)

The request gave us the following response data back in a [`JSON`](https://en.wikipedia.org/wiki/JSON) format. As you can see, some of the values in the response relate to the `query parameters` used to filter the response of the `/booking` endpoint. Hopefully, now you can see why the response data was changing each time you made a request.

```json
{
    "firstname": "Sally",
    "lastname": "Jackson",
    "totalprice": 898,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2016-10-25",
        "checkout": "2018-02-27"
    },
    "additionalneeds": "Breakfast"
}
```
The nice thing about Postman is that it displays the response data in `Pretty` print by default and this is awesome, for me, it makes it so much easier to read. If you would like to see the response displayed a different way, Postman has a couple of options for you - `Raw` and `Preview`. Switch between this options to see the data in these display modes. The clip below shows how this looks in the application for the response data above.

![Response Display](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/06_getSingleBooking/Response_Display.gif)

### Using Request Headers

In the last example we looked at using [`Preset Headers`](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/04_preSetHeaders.md) but we didn't actually use them to make a request to the Restful-Booker API. Fear not, that's about to change. We are going to be using the `Accept` header to change to way that the response data is returned - There are two different response data types for this endpoint `application/json` and `application/xml`. The default seems to be `application/json`, if a header is not supplied in the request.

Adding a new header is straight forward enough:

- In the Request Builder view, select the `Headers` tab
- Add `Accept` in the `Key` text field
- Add `application/xml` to the `Value` field next to the key

You will notice as you type into the fields, there is a handy auto-complete feature, this displays several options available to you.

![Add Request Header](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/06_getSingleBooking/Add_Request_Header.gif)

With the `application/xml` header set, you can see that the following response is sent back:

```xml
<?xml version='1.0'?>
<booking>
    <firstname>Sally</firstname>
    <lastname>Jackson</lastname>
    <totalprice>898</totalprice>
    <depositpaid>true</depositpaid>
    <bookingdates>
        <checkin>2016-10-25</checkin>
        <checkout>2018-02-27</checkout>
    </bookingdates>
    <additionalneeds>Breakfast</additionalneeds>
</booking>
```

A couple of quick things that I have noticed when using the `application/xml` header in the request, Postman tries to automatically match the response data type. It looks like that the API is returning, what looks to me like valid `xml`, but the validation has failed for some reason...This is a perfect opportunity to explore this further.

![Response Validation](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/06_getSingleBooking/Response_Validation.PNG)

Looking at the `Response Headers` - This is an area we will cover in more detail soon, we can see that the response `Content-Type` is `text/html`...There is clearly something strange happening here, it would need more time to dig into this issue more by either setting up a proxy tool like [Fiddler](https://www.telerik.com/fiddler) or even better, pairing up with the developer to investigate the issue together. I'm not going to go down that route now as it will shift the focus away from Postman.

![Response Headers](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/06_getSingleBooking/Response_Headers.PNG)

Before moving on to do a quick introduction to the `Pre-request Script` feature, I'm always looking for ways to bend the rules that I've seen explicitly written in some documentation. As Testers, we always seem to be on the look out for `What if..` scenarios, Request Headers are absolutely something that can be explored. So what do we know? We have been told that their are two types of headers that can be used but we know that there are so many more available...What if...I used a different one?!

![Use Another Request Header](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/06_getSingleBooking/Use_Another_Request_Header.gif)

That's an interesting response back when using a different `Accept` header - `418 I'm a teapot` :)

### Dynamic Requests

> "Pre-request scripts are snippets of code associated with a collection request that are executed before the request is sent. This is perfect for use-cases like including the timestamp in the request headers or sending a random alphanumeric string in the URL parameters."

This is probably my favourite Postman feature, I'm going to be spending a whole bunch of time creating future examples for this feature because it can be used in so many useful ways. For this introduction, I'm going to show you how we can use it to make our requests to the `/booking/{id}` endpoint slightly more dynamic. The default total number of booking id's returned from the `/booking` endpoint is _normally_ `10`. What we are going to do is use one of the built in [Lodash](https://lodash.com/docs/4.17.10#random) functions to create a random number between 1 and 10, then use that value in our request. I'm going to be covering the built in modules it a later example so don't worry if this is a little bit confusing. :)

```Javascript
_.random(1,10)
```

To show you what's going on with this `_.random()` feature, I've logged out the created value, in the Postman Console. As you can see, the numbers returned are all between `1` and `10`. The numbers that you pass the `_.random` function are the lower and upper ranges so this could be used to create any range that you need. A really handy feature!! :)

![Console Log Random Number](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/06_getSingleBooking/Console_Log_Random_Number.PNG)

The `Pre-request Script` feature is basically just a little JavaScript IDE...ish, the code above is simple and it just generates a random number. There are lots of handy JavaScript tutorials where you can learn the basics on the [W3schools](https://www.w3schools.com/js/default.asp) website. We have the code to _create_ the value so how can we use this in a request?

Postman has a bunch of built-in code `Snippets` that you can use - We are going to be using the `Set an environment variable` snippet. This will place a `key/value` pair of data into our _Restful_Booker_Environment_ file. The `Snippets` are listed down the right hand side of the request data builder.

![Use Snippet](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/06_getSingleBooking/Use_Snippet.gif)

From the clip above you can see that we have selected the `Set an environment variable` snippet and this has been added to our pre-request script window. We then give our new variable a `name` as this is the `key` that we will be referencing in our request, I've called mine _booking_id_. Finally, the JavaScript code to create the random number is added as the `value`. Simple right?! Now let's start using it in our request.

In the request URL, we can use the `{{booking_id}}` syntax to set the variable value and send our request to the API endpoint. You can see that the variable is set for the first time (It changes colour from red to orange), just before the request to the API is sent.

![Dynamic Request](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/06_getSingleBooking/Dynamic_Request.gif)

That's the end of this example. We have learnt how to use request headers to change the data in the response and we have also taken a super quick look at pre-request scripts. More to come on those very soon, It's a great feature of Postman and I want to write a few single examples on that feature to do it justice.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
