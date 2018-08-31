# Importing Files

In this example we will look at some of the methods of importing various different files into Postman.

---

During these examples we will be building up a Collection of requests, the most up to date Collection will live in the [Collections](https://github.com/DannyDainton/All-Things-Postman/tree/master/Collections) directory. Sharing Collections with different people is an easy way to get people, both internal and external, up to speed quickly and making requests to an API.

We can import a Collection file by following the steps below:

- Press the `Import` button, you can find this in the top left of the application  
- In the `Import File` tab, Either drop the collection file into the grey shaded area or navigate to the file on your machine using the `Choose Files` button
- A message will be displayed to confirm that the file has been successfully imported

If everything has gone to plan, you should be able to see and use the Collection. See below for a quick clip showing the process.

![Import Collection](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/05_importingFiles/Import_Collection.gif)

The process above can also be followed to import `Environment` files into Postman. You can find the latest iteration of these, from the example, in the [Environments](https://github.com/DannyDainton/All-Things-Postman/tree/master/Environments) directory.

Environment files can also be imported from the `Manage Environments` section using a similar `Import` feature. Let's look at that process, it's slightly long winded but you will achieve the same results as the steps listed above.

- Press the `cog` icon in the top right on the application, just above the `Save` button
- Press the `Import` button
- Press the `Choose Files` button to select a file from your machine
- Once chosen, select the `Open` button to import the file
- You will receive a notification to confirm the successful import

![Import Environment](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/05_importingFiles/Import_Environment.gif)

Once complete, you will see the new environment file and be able to use this within your Collections.

### It's not just about Postman specific files

The `Import` feature doesn't only just accept Postman created files, we can import [Swagger](https://swagger.io/), [RAML](https://raml.org/) and [WADL](https://en.wikipedia.org/wiki/Web_Application_Description_Language) files but also [cURL](https://en.wikipedia.org/wiki/CURL) requests.

![Import Feature Options](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/05_importingFiles/Import_Feature_Options.PNG)

If we look at the example request below, this is a very basic `POST` request which has a couple of request headers and a simple JSON body. When imported, Postman will convert all the values from the request and feed these into the different sections within the application. This can be really handy if you want a user interface wrapper around a normally command line based request.  

```bash
curl -X POST 'https://my-request.com/myRoute?myKey=myValue&myOtherKey=myOtherValue' -H 'accept: application/json' -H 'authorization: Bearer 123456' -H 'content-type: application/json' -d '{ "message": "This is a new message" }'
```

This is a clip of that process in action:

![Import cURL Request ](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/05_importingFiles/Import_cURL_Request.gif)

You can also use the `Paste Raw Text` section of the `Import` feature, to paste a raw cURL request into Postman, bypassing the need to add request this to a file beforehand. The clip below shows this type of import in action.

![Import raw cURL Request ](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/05_importingFiles/Import_Raw_cURL_Request.gif)

This is a very basic look at importing specific files into the application. We have learnt how to import Collections and Environment files in to Postman but also took a quick look at importing some non Postman based files and requests.

In the up coming examples, we will look at the different ways that we can `Export` different files for other people to use within their Postman instance.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
