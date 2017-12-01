# Get all Bookings

For this example we will be making requests to the `/booking` route and using some different parameters in the requests to filter the response data.

---

### Collections management

Before we start making our requests to the `/booking` route, We're going to walkthrough how to carry out some Collections housekeeping. Having a single collection for _All_ of your requests can just get messy fast. In Postman, we can create our very own folder structure within a collection and separate out our requests into a more meaningful order. I don't really have a one size fits all solution for this - I'm going to go with what _I_ think feels right for example purposes but it can always be changed. As long as you're comfortable with the process of creating and ordering them, I'm happy! :)

Creating a new folder in a Collection is very simple, by following the steps below you can create your own:

- Hover over the main `Restful_Booker_Collection` and an `ellipsis` icon will appear
- Press the `ellipsis` icon to display the context menu
- Select the `Add Folder` option
- In the dialog box, Give the folder a name (Mine is called _Health Check_)
- Finally, Press the `Create` button

![Create Folder](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/03_getAllBookings/Create_Folder.gif)

You can move requests around by dragging and dropping them into specific folders.

![Put Request In Folders](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/03_getAllBookings/Put_Request_In_Folder.gif)

Another method is to save them directly into a folder, I've created a _Get all Requests_ folder and we will save our first `/booking` request into this new folder.

- Press the `Save` button to the right of the `Send` button
- In the dialog box, Give the request a name (I've named mine _Get all bookings_)
- Using the `Search` field, you can find the _Get all Requests_ folder
- Select the desired folder from the list of results
- Finally, Press the `Save to {folder name}` button

![Save Request To Folders](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/03_getAllBookings/Save_Request_To_Folder.gif)

Once those steps are complete, the new requests will be saved into the _Get all Requests_ folder. Okay, so I think that's all the housekeeping for now, we'll now move forward and start making some requests.

### Getting the Booking data

Looking at the documentation for the Restful-Booker API, we have a couple of new things that we can use to explore the data returned. In most API endpoints you can use query string parameters to specify options that limit the data included in responses. Our first request to the `/booking` route will bring back _All_ the data within the system.

![Booking Request](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/03_getAllBookings/Booking_Request.gif)

Looking closer at the response data we can see that this returns an array of `Booking Ids`. Looking ahead at the documentation, I can see what these Id numbers relate too but we're going to stick with just this endpoint on this example.

:question: As we test the API we will undoubtedly have questions - Mine will never be an extensive list of questions, that's not my main focus for the examples but I will always note a few down along the way. :)

- Should this be the `/bookings` route? It returns _all_ entries so should the route name be plural?
- The ordering of the numbers returned, should these be in a sequential order (1, 2 , 3, etc.)?
- Could the response structure be less repetitive? One `bookingid` key and a list of Ids?
- Should the `key` be using Camel Case syntax? E.g. `bookingId`
- Lots more...

To be honest, these are questions that could have been easily asked during the creation of the API, rather than retrospectively. That's why it's vital to continuously communicate during the whole development process.

Let's move forward and look at the different ways that we can filter the response data using the query string parameters available.

![Booking Parameters](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/03_getAllBookings/Booking_Parameters.PNG)  

We have a few to _play_ with here `firstname`, `lastname`, `checkin` and `checkout`. As per the documentation's example, these are used in the following way within the request:

`/booking?firstname=sally&lastname=brown`

You can see the route `/booking` after this we have the `?` which starts the query, this is followed by the parameter `firstname` which `=`'s a value, in this case it's `sally`. Parameters can be chained together using `&`, this can be used to refine the data returned within the response.

In Postman, we can enter these parameters in two different ways, written directly into the response or using the `Params` feature. The first way is _probably_ the easiest because you could paste the URL straight into the request field.

![Parameters Directly In Request](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/03_getAllBookings/Parameters_Directly_In_Request.PNG)

The second way is using the very handy `Params` feature, this is really cool as it constructs the query string for you, based on the entered values.

![Params Feature](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/03_getAllBookings/Params_Feature.gif)

### Explore It!

So while using the API, I'm always thinking of multiple different ways that I can explore what's in front of me, to highlight potential issues.

One of my go to resources and something that has uncovered more issues that I can remember is the [Test Heuristics Cheat Sheet](http://testobsessed.com/wp-content/uploads/2011/04/testheuristicscheatsheetv1.pdf) From [Elisabeth Hendrickson](https://twitter.com/testobsessed)

The main piece of `Testing Wisdom` I always keep in my mind is this:

 > "It’s all about the variables."

From the API documentation we already know lots of valuable information about this endpoint and a number of variables that we can use to our advantage while exploring the `/booking` endpoint. We've been told that the query parameter types are `Strings` and `Dates` in the `CCYY-MM-DD` format. Using the two sections below, extracted from the cheat sheet, test the endpoint by changing and manipulating the parameters before sending the request to discover new information.  

![THCS Strings](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/03_getAllBookings/THCS_Strings.PNG)

![THCS DateTime](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/03_getAllBookings/THCS_DateTime.PNG)

I don't want to list out all the issues that I've discovered while testing. This is a chance for you to interact with the endpoint and have fun while exploring within a safe environment.

### Some basic tests

Using the `lastname` parameter and the example given in the documentation, I sent a request off and the response back was an empty array. I know that the value is a `name` so maybe the first letter is a Capitol e.g. `Brown` - This time a single entry was returned, this is telling me that the search functionality is not case insensitive.

![Case Insensitive](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/03_getAllBookings/Case_Insensitive.gif)

Another issue found was around validation of the Date - I decided to break the rules by seeing how it would handle a 13th month in the date value, this resulted in a `500 Internal Server Error`. Oh dear! :(

![Date Rules](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/03_getAllBookings/Date_Rules.gif)

These are just a couple of basic tests but I'm sure you can have some fun exploring this endpoint and highlighting some more issues :)

That's the end of this example. We have learnt how to organize our `Collections` by using a folder structure and also sending requests using query string parameters. More to come very soon.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
