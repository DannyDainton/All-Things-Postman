# Create an Environment File

In this example we will be creating an Environment file and using data from this file within our requests.

---

In this second example I’d like to introduce you to `Manage Environments` this is one feature that I use a lot, you can store pieces of information that can be used in a number of different requests.

In Postman, we can create storage files to hold variables. These can be used in a specific `Collection` or they can be environment agnostic and used globally, on any request.

We’re going to concentrate on the specific environment approach first and learn more about global variables in later examples. With most APIs, the `routes` will predominantly be the same no matter where that service is running. The only thing that _normally_ changes between environments is the base path, in the previous example we made a request to the `/ping` route and shortly we'll be making requests to the `/booking` route in the next example, these both use the same base path `https://restful-booker.herokuapp.com`. During the course of the APIs development, it would _usually_ be developed and tested on multiple different environments before reaching production.

To bring this more to life, I've mocked the API using a tiny [Nodejs Express API](http://expressjs.com/). Our `Development` environment would have a base path of `dev-restful-booker` and our `Staging` environment would be `staging-restful-booker`, however, the route would be same everywhere.

Let's see an clip of this in action:

![Different Environments](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/02_createEnvironmentFile/Different_Environments.gif)

You can see that the `/ping` route is the same in both requests but the base path is different. So this is where the Postman `Environments` come into action. It's just a pain to manage separate collections for each environment, one central configurable file makes much more sense! :)

This is jumping ahead slightly and we will be creating our own environment file and referencing the data values shortly but I wanted to show you a quick clip of the magic first:

![Switching Environments](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/02_createEnvironmentFile/Switch_Environments.gif)

In the clip, you can see that the only thing that is changing is the environment file being used for the requests. How about we create our own file and start using the syntax to reference the data.

### Creating our first environment file

A new environment file can be created in a number of different ways. One way is by following the steps below. In the file, we will be setting the `baseURL` value to `https://restful-booker.herokuapp.com` and using this in all of our requests as we move through the examples.

- Press the `cog` icon in the top right on the application, just above the `Save` button
- Press the `Add` button
- Give your environment file a name (I've called mine _Restful\_Booker\_Environment_ )
- In the `Add a new variable` field, type `baseURL`
- In the field next `baseURL`, type `https://restful-booker.herokuapp.com`
- When you're done, Press the `Add` button
- To get back to the main window, press the `X` button

This process can be seen in the clip below:

![Create an Environment file](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/02_createEnvironmentFile/Create_An_Environment.gif)

An alternative way of creating a new file is by using the `New` button, this can be found in the top left of the application.

- Press the `down arrow` on the right side of the `New` button
- Select the `Environment` option, this will open the `Manage Environments` feature on a new file ready to be edited
- Complete the fields with the information you would like to use

The clip below shows this process, I've used fake data this time as we have already created our file using the previous method.

![Alternative Create Environment ](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/02_createEnvironmentFile/Alternative_Create_Environment.gif)

### Referencing data from the file

In order to use the data within the file that we just created, we need to reference this in the request. Postman uses a double curly brace at either end of the `key` to make the link to the `value`. The syntax looks like this ```{{myKey}}``` and this syntax can replace the `https://restful-booker.herokuapp.com` value that we already have in our request.

![Set an environment value](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/02_createEnvironmentFile/Set_An_Environment.gif)

When you add the final curly brace to the request, you will notice that the colour changes to `red` this is Postman telling you that it knows that you want to reference something but it doesn't recognise what that thing is....yet! :)

By hovering over the `red` value, you will see that it displays a tool tip with an `Unresolved Variable` message:

![Unresolved Variable](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/02_createEnvironmentFile/Unresolved_Variable.PNG)

Let's link this all up - Select the _Restful\_Booker\_Environment_ file from the dropdown list in the top right of the application. This will have a default value of _No Environment_ until one is selected or if none have previously been created.

![Use an environment file](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/02_createEnvironmentFile/Use_Environment_File.gif)

Once the _Restful\_Booker\_Environment_ file is selected, you will notice that the `red` value has now turned `orange`. If you hover over this value, you will see in the tooltip the variable being set from the environment file. Awesome! We are now ready to make our first request using data within an environment file...exciting! :)

![Request with an Environment file](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/02_createEnvironmentFile/Request_With_An_Environment_File.gif)

That's it, we've completed this example - This is still the basics, the environment files can be used for many different things, which we will cover soon but just getting used to creating them and using the values from them is a great first step. More to come soon!

### Finally...

The eagle eyed amongst you would have noticed an `orange` dot in the request tab, this is Postman being awesome and telling you that you have unsaved changes. You can save these changes, if you want to, by pressing the `Save` button to the right of the `Send` button. If you accidentally close the tab with unsaved changes, you will get a warning asking if you would like to save the changes before leaving. Phew! :)

![Unsaved Changes](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/02_createEnvironmentFile/Unsaved_Changes.PNG)

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
