# Ping the API

In this example we will make a request to the `/ping` route and examine the response data.

---

A simple route such as `/ping` is a cheap way of providing quick feedback about the status of the API. Depending on the type of the API, you may not have consumers requesting data 24/7 so how can we be sure that the API is still alive? We use a similar method in my team that we display on a Grafana dashboard (See image below) - This has an alert attached to the metric so we can be notified of any potential problems.

![Grafana Heartbeat](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/01_pingTheAPI/Grafana_Heartbeat.PNG)

The grafana dashboard is one way of displaying a health check status but a more basic form can be achieved right in the browser. The example below is just a very crude and simple way that a health check 'ping' could work.

In Chrome, Press `F12` to display the developer tools, Using the `Console` tab you can paste in the basic JavaScript code below and it will create the `IsItAlive` variable. The code snippet below is using [jquery syntax](https://api.jquery.com/jQuery.get/#jQuery-get-url-data-success-dataType) that is supported in most browsers.

```javascript
const IsItAlive = () => {
    $.get("https://restful-booker.herokuapp.com/ping", (data, status) => {
        console.log("Response: " + status)
        })
    }
```

By using the `setInterval` method and passing in a duration (2 seconds in the example), we can poll the API to check its status. A very basic API health check tool running in your browser. Simple.

```javascript
setInterval(IsItAlive, 2000)
```

To stop this we can use the `clearInterval({process Id})` method in the console. The process Id would have been placed in the Console when the polling started.

See an example of this in action below.

![Health Check](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/01_pingTheAPI/Health_Check.gif)

---
### Let's get started with Postman

So before we really get going (honestly, we'll start soon) and this is just a personal preference - Staring at white backgrounds all day long makes my eyes go all funny so I tend to opt for a darker background if the tool provides this, as Postman is awesome, you can switch to a darker theme but it's completely up to you. Here's how to switch:

- Press the `Settings` icon, this is the spanner looking one in the top right of the application
- Select the `Themes` tab
- Switch to the Dark Theme

![Change Theme](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/01_pingTheAPI/Change_Theme.gif)

#### Making our first Request

By default, the tool opens all new requests with the `GET` verb and as this is what we're going to be using first, we're good to go. :)

Enter the base URL of the Restful-Booker API `https://restful-booker.herokuapp.com` followed by the route that we would like to use `/ping` into the URL input field at the top of the tool. Press `send` or use the shortcut `CTRL+ENTER`. Congratulations, you've just sent your first request to the API.

![Ping Request](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/01_pingTheAPI/Ping.gif)

#### The Response data

![Ping Response](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/01_pingTheAPI/Ping_Response.PNG)

From a very simple request we can gather lots of useful information about response times, response data size and tons of details in the Headers (which we will cover in later examples). We're going to just take a look at the `response code`, this can be found in the red box on the image above.

According to the documentation provided, we should be seeing a `201 Created` response back from our request.

That's the very first example complete - I've purposely started very basic and the pace might not suit people that already have a good understanding of the tool already but this will become more advanced very soon so please stick with it. :)

### One more thing
##### Create a Collection

> "A Postman Collection lets you group individual requests together. These requests can be further organized into folders."

As there is going to be a whole bunch of examples within this repository, let's create a Collection for all the lovely requests we'll be making. There are a few different ways of creating these, the quickest way is selecting the `New Collection` icon which can be found under the `Collections` tab.

![Create Collection](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/01_pingTheAPI/Create_Collections.PNG)

This will present you with a new dialog box where you can enter the name you would like to give the collection. That's it! :)

The easiest way that I have found is by adding the request that you have just made to ping the API, to a new collection. Here's how to do this:

* Press the `Save` button to the right of the `Send` button
* Give the request a name (This will default to the request URL but it's good to give it a more meaningful name)
* Select the `+ Create Collection` option
* Name the Collection (I've named mine _Restful_Booker_Collection_) and press the `tick` icon
* Finally, Press the `Save to {Collection Name}` button

See below, this will walk you through this process:

![Create a Collection](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/01_pingTheAPI/Restful_Booker_Collection.gif)

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
