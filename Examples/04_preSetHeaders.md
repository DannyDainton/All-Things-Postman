# Preset Headers

For this example we will be looking a bit closer at the `Preset Headers` feature. This will be concentrating on the feature as a stand alone item, we will be working with `Request Headers` in the next example but I wanted to highlight this as particular feature that could save you some time.   

---

Who likes to do the same repetitive things over and over?! Not me. That's why I'm a fan of the `Preset Headers` feature in Postman. _Most_ of the requests that we will send will have a selection of `Headers`. These can provide Authorization details, Accepted data formats, Valid API Versions, etc. We'll be using some of these in future requests.

In order to get to the feature and start creating Presets, we need to open it up. This can be done by the following these steps:

- Select the `Headers` tab in the `Request Builder` section
- Press the `Presets` dropdown to the right of the application, under the `Save` button
- Select `Manage Presets`, this will display the `Manage Header Presets` dialog box
- Press the `Add` button to create a new Preset

![Open Pre Set Headers](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/04_preSetHeaders/Open_Pre_Set_Headers.gif)

Once the steps above are followed, we can start adding data to our preset.

- Give the header preset a `name` (I've called mine _Restful_Booker_Preset_Headers_)
- Add some headers of your choice into the fields as `Key/Value` pairs
- Once you're finished, Press the `Add` button

Here's an example of what I've added to mine. As you can you see, the Preset values can contain `variables` using the ```{{myVar}}``` syntax, we learnt how these work in a previous lesson.

![New Presets](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/04_preSetHeaders/New_Presets.PNG)  

There's a handy auto-complete feature within the `Headers` section, this will present you with a pre-defined list containing _most_ of the commonly used request headers. The clip below shows this feature in action...very cool! This feature is also available when creating your pre-sets headers. 

![Header Auto Complete](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/04_preSetHeaders/Header_Auto_Complete.gif)

### Using Bulk Insert

As much as the Presets save you time, you're still entering the `key/value` pairs one by one. Thankfully, these can be bulk loaded as plain text, as long as the value is separated by a [Colon](https://en.oxforddictionaries.com/punctuation/colon). The text below shows the same values as I added one by one previously as `key/value` pairs - Let's look at how to bulk enter these values.    

```
accept:application/json
content-type:application/json
Authorization:Bearer 123456
myCustomKey:{{myCustomValue}}
```

- Open a `Preset` in the `Manage Header Presets` dialog box by selecting it from the list or creating a new one
- Press the `Bulk Edit` button, this will give you a text area with some usage instructions
- Paste the example text from above into the text area
- Select the `Key-Value Edit` option to see how the headers look in that format
- If you're happy, Press `Add` or `Update`, depending if you're creating or editing a existing Preset

![Bulk Add](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/04_preSetHeaders/Bulk_Add.gif)

### Using the Preset Headers

So we have some shiny new Preset Headers created and now we'd like to start using these in the requests that were making to our API endpoints. This is very simple...

- In the `Headers` section of the `Request Builder`, Press the `Presets` dropdown
- Select the Headers that you would like to use in the request (We will use the _Restful_Booker_Preset_Headers_ Preset)
- Once selected you will see the headers populated in the main `Headers` section

![Using Presets](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/04_preSetHeaders/Using_Presets.gif)

That's a quick introduction to the Preset Headers feature, in the next examples we will start using Headers within our requests to the Restful-Booker API.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
