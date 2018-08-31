# `POST` /booking

In this example we'll be using the `POST` method to send data to the `/booking` endpoint and create some new bookings based on the information we provide in the request.

---

This is the first time we will be using the `POST` method. Up to now, the examples have all been using the `GET` method to do exactly that - We were sending a request to get different pieces of data back from the Restful-Booker API endpoints. This wasn't changing the data in any way but by using the `POST` method, we will be creating new `bookings` based on the data we send in our request.

![Post Body](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/10_createNewBookings/Post_Body.PNG)

From the [Restful-Booker documentation](https://restful-booker.herokuapp.com/apidoc/index.html#api-Booking-CreateBooking), we can see that a new booking can be created using either a `JSON` or `XML` payload. We also need to add the correct `Content-Type` header to the request depending on the payload format we are sending.

### Create a new bookings

In order to add data to a request, we need to access the `Body` section of the request builder - This option will be inactive for the `GET` method and will only become active when we select the `POST` method.

![Active Request Body](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/10_createNewBookings/Active_Request_Body.gif)

We can now add our request data - Postman will automatically try and set the correct `request header` when using the `raw` option - This is based on the choice that you make in the dropdown, we need to select `JSON` so it will set `application/json` as the `Content-Type` in the Headers section.

![Set Header](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/10_createNewBookings/Set_Header.gif)

By setting this as `JSON` it will then change the `raw` input field, to validate the data entered. This will give you some quick feedback and highlight any issues before any request is made, which is really handy. In the image below, I removed a comma from the example data set and this error is highlighted in the UI. If you see any of these validation errors, hovering the cursor over the `X` icon will give you more context about the actual error.   

![JSON Validation](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/10_createNewBookings/JSON_Validation.PNG)

Using the sample data set from the documentation, we can see the structure and format of data that is used to create a new booking. First off, let's see what happens when we send through the request without making any changes to the data - We are looking to see what the response looks like:

Request data:

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

![Create Booking](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/10_createNewBookings/Create_Booking.gif)

Response data:

```json
{
    "bookingid": 12,
    "booking": {
        "firstname": "Sally",
        "lastname": "Brown",
        "totalprice": 111,
        "depositpaid": true,
        "bookingdates": {
            "checkin": "2013-02-23",
            "checkout": "2014-10-23"
        },
        "additionalneeds": "Breakfast"
    }
}
```

We can see a number of things just from the response - A `bookingid` has been returned, we know that we can get details of a single booking from the `/booking/{id}` endpoint so that's something to explore further. The format returned in the `booking` object is slightly different from the format we used in the request, is that a problem? Should this be consistent? We can also see that the `status code` returned is a `200 OK` - Should this be changed to return a `201 Created`?

From such a basic request we are starting to create a list a different questions and we haven't really _tested_ the endpoint yet. When I'm looking at the sample data, I'm looking for the variables that can be changed and creating a list of _What if's_ in my mind.

- What if I change the numbers for strings?
- What if the checkout date is before the check in date?
- What if I leave some of the data out completely?
- Is there a limit of digits we can use on the `totalprice` property?
- I'm thinking about how this data is being stored, what would prevent this from being saved correctly?

To name a few but I have loads more going through my mind - What are the _What if's_ that you would think about - Try them out and explore this endpoint.

I'm going to just explore a couple of those _What if's_ - Firstly, What happens if you remove an item from the data sent in the request? I think the `firstname` is an important one, there is nothing telling me that this is required...so let's remove it and see what happens...

![Missing Data](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/10_createNewBookings/Missing_Data.PNG)

This causes a `500 Internal Server Error` which is obviously not a good thing - When something like this happens, I think about the [`FAILURE`](http://www.questioningsoftware.com/2007/08/failure-usability.html) mnemonic and how the error, that I'm seeing, lines up against each word in the mnemonic. If the data is required, can that be made visibly to the user? We could either do this by writing the details in the API documentation or also by giving the user feedback in the response - Make this action a `400 Bad Request` with a response message stating _why_ this is happening, this gives the user more information to work from to solve the problem, rather than a generic server error message.

In the second scenario, I want to explore the `totalprice` property to see what information I can get from this value. I start to increase the number of digits in increments of 2, for no other reason than to control the test data. This starts to get _interesting_ when I get to 15 digits, on the 16 digit the value changes to something I wasn't expecting to see.

With 15 digits:

![Fifthteen Digits](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/10_createNewBookings/Fifthteen_Digits.PNG)

With 16 digits:

![Sixteen Digits](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/10_createNewBookings/Sixteen_Digits.PNG)

Taking this slightly further, when you enter 21 digits and send the request - It just gives up a returns to 1.

![Twenty-One_Digits](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/10_createNewBookings/Twenty-One_Digits.PNG)

How about providing a string value and not a number - How is this handled...

![Break Convention](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/10_createNewBookings/Break_Convention.PNG)

Those are basic tests that just sparked my curiosity and can be run quickly to uncover information about the API and how it handles different requests. Use the Restful-Booker API to run your own tests and see what you can discover.

That's the end of this example, we have learnt how to use Postman to make a `POST` request to the API and create new a new booking based on the information provided. In the next example we will be looking at how to remove the manual data entry aspect away and use Postman to create this data for us.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
