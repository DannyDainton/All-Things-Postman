# Introduction to the collection runner

In this example we will be looking a bit closer at the `Collection Runner` which allows us to run multiple requests.

---

The `Collection Runner` is where things start to get a whole lot more interesting - It's where you will start to automate the running of the requests that you have built up in the Collection folders. With the click of a single button, you will see all the requests execute before your very eyes.

The `Collection Runner` itself offers loads of different features and we will be moving on to them in a later example but for now, we will just go over the basics, to get you going and become more familiar with that part of the Postman application.

### Where does this automated magic happen?

With Postman always being super useful, It provides you with multiple options for opening the `Collection Runner`. Each person will have their own method of doing this, it's completely up to you, they all ultimately achieve the exact same thing.

On the main application view there are a series of buttons in the top left of the screen, by selecting the `Runner` button, this will open the `Collection` Runner in a new window. To the right of this button you will see a `file` type icon, selecting this will give you a dropdown list of options - Select the `Runner Window` option to open the `Collection` Runner in a new window.

![Postman Runner](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/12_introductionToTheCollectionRunner/Postman_Runner.PNG)

Another method of accessing the Runner, is straight from the Collection folder itself.

![Open Runner](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/12_introductionToTheCollectionRunner/Open_Runner.gif)

By selecting the arrow icon at the side of the Collection folder, this opens a new panel to the right of that folder. Select the blue `Run` button - This will open a new window, the Collection folder will be the collection in view on this newly opened window. This opening method is slightly more targeted than the rest, as those other methods all open the Runner with _all_ the Collections in view so there are a couple extra steps involved to get to the one you require.

Finally, for those people who have no time to waste clicking buttons on the UI - The Runner can be opened by simply using the keyboard shortcut `Ctrl+Shift+R`.

### Running a Collection

To keep this a very basic introduction to the Runner, we're going to use the _Restful_Booker_Collection_ that we have been building up during these examples. This `Collection` contains lots of different requests with a few basic `Tests` included - This is going to be perfect to demonstrate the core functionality of the `Runner`.

With the `Runner` open in a new window, we can select the `Collection` that we require from the list available. If you only have a couple of Collections created, it's easy to just select the one that you want to run from the list but if you have multiple Collections, you can use the `Search` feature to filter out the unwanted Collections. As you start to type, you will start to see the list being auto-filtered. Once you see the one you require, select this and you will see the sub-folders/requests contained within the Collection.

So we have that Collection ready to go....but first we need to set the `Environment` file, just like if we are sending our requests from the main application, we have added those `{{...}}` syntax placeholders in multiple places - If we were to start the Runner without setting the `Environment` file first, this wouldn't really know what variables we are trying to reference so it would fail straight away.

Setting this is simple, similar to how we set the `Environment` file in the main application, we just simply select it from the dropdown menu in the Runner and we are good to go!

![Set the file](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/12_introductionToTheCollectionRunner/Set_The_File.gif)

We have a couple of other options open to us on the `Runner`:
- Iterations
- Delay
- Log Responses
- Data
- Persist Variables

For this example will we take a look at the `Iterations`, `Delay` and `Log Responses`. The `Data` option is something that we will cover in a later example, this gives us the ability to drive the requests using data from an external `JSON` or `CSV` file - That is super useful and we'll be looking at the soon.

The `Persist Variables` checkbox will do just that if you have it ticked, this will change all the `Environment` and `Global` that you're using in the Collection run, which is not great if you're running the Collection multiple times and you want to reuse the same variables in your requests. By default, `Persist Variables` is checked the first time you open the Collection Runner. If you do not want variables to be updated during the run, deselect the `Persist Variables` checkbox.

The `Iterations` option is hopefully quite self explanatory, This setting determines the number of times the selected Collection or sub-folder will be executed during the run. It's default setting is `1` so that will just run through the Collection once. By increasing the value in the `Iterations` option, we will tell the Runner to run the Collection `X` amount of times.

In the clip below, you can see that the `Iterations` value has been set to `5` - This is telling the Runner to execute the requests in the Collection/Sub-folder five times.

![Multiple iterations run](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/12_introductionToTheCollectionRunner/Multiple_Iterations_Run.gif)

The `Delay` option will add a wait time between each requests execution, this value is `milliseconds` so if we add `2000` this will add a 2 second delay between each request in the Collection. The runner executes each query in the collection instantly so if you need to add some extra time for a downstream action to happen before a subsequent query is run, this feature gives you that option.

In the clip below, the `Delay` value has been set to `3000` milliseconds so this will place a `3` second delay or wait time between the requests being executed.

![Delayed run](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/12_introductionToTheCollectionRunner/Delayed_Run.gif)

The `Log Responses` option allows you to view the request/response bodies and headers for your requests in the `Run Results` section, we will be taking a look at what this looks like next so I won't go into any real detail here. This is a great option for debugging the requests that have been run but it can also impact performance, if you're running larger Collections.  

So let's run our first Collection in the `Runner` - We will select the _Restful_Booker_Collection_ from the list of options and set the _Restful_Booker_Environment_ file then we'll start the run.

![Run the collection](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/12_introductionToTheCollectionRunner/Run_The_Collection.gif)

As you can see from the clip, running small Collections is pretty quick. Not only did the `Runner` run multiple requests in one click, we also run our `Tests`. On this occasion, we checked 18 different things that we had specified in our requests.  

### Checking the Results

Once the Runner has worked its way through our Collection of requests we're presented with the results of that run. The section contains lots of information that I will run through but to not completely blitz your minds, I'll save some of the features like `Export Results` for a later example.

![Collection run summary](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/12_introductionToTheCollectionRunner/Collection_Run_Summary.PNG)

We have several places that give us a quick visual indication of the state of the Collection run, In the top left of the `Run Results` section we have a breakdown of the number of `Tests` that have been run and a Passed/Failed summary. Along with this test summary, we can see the name of the `Collection` that has been run and the name of the `Environment` file that has been used during that run.

![Test summary](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/12_introductionToTheCollectionRunner/Test_Summary.PNG)

The `Run Results` section will also tell you which `Tests` were run against which requests and the out come of these, be it Passed or Failed.

![Tests executed](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/12_introductionToTheCollectionRunner/Tests_Executed.PNG)

We mentioned the `Log Responses` option briefly, this is a great feature and helps us to drill down into the Collection Runner requests. In the main Postman UI it's easy to see the response headers and body when making single requests but it's always been slightly more difficult to see this as part of a Collection run.

Now we can - In the `Run Results` section, select a request by clicking on the name, this displays a dialog box with different pieces of information all about the request. The URL and the Headers and Body for both the Request and the Response can be viewed here, if you have selected that the `Runner` logs responses for all requests.  

A example of the information that you can get from this feature can be seen below, this has been very useful when dynamically creating data to send in a POST request - It gives you information on exactly what's been sent through in the request.

![Request information](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/12_introductionToTheCollectionRunner/Request_Information.gif)

Each Collection or Sub-folder that has been run will appear in the `Recent Run` list, each of these runs can be selected and the information will be displayed in the `Run Results` section.

From the `Recent Runs` section, we can delete the runs that we no longer need to see in the list. This is done by simply highlighting the run and clicking the `rubbish bin` icon. This deletes the run straight away so ensure that this is want you want to do.

![Delete run](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/12_introductionToTheCollectionRunner/Delete_Run.gif)

So that's it for this example, this was a very brief look at the `Collection Runner` and give you an insight into how you can run multiple requests in a single button click. We will be starting to look closer at the Runner in the next examples and seeing how we can use `JSON` and `CSV` files to inject data into our requests.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
