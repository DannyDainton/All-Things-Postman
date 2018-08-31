# Global and Dynamic variables

In this example we'll be looking at how to `Create`, `Use` and `Clear` global variables. We will also be taking a look at the Dynamic Variables that Postman offers.

---

Previously, I wrote an example all about the `Manage Environment` feature - This gave us a method of storing different pieces of data as `variables` within a single environment. These can then be used in multiple areas on a request, all under the same environment umbrella.

That's perfect if you have specific environments but maybe you have the need to set a variable that can be used anywhere, regardless of environment, that's where `globals` come into to play. This gives you the ability to set a value globally and reference this data everywhere.  

Let's take a closer look at how these work and how we can `set`, `get`, `unset` and `clear` them in the application.

Like the `environment` variables, the `global` variables can be set using the UI, we can do this by following these steps:

- Press the `cog` icon in the top right on the application, just above the `Save` button
- Press the `Globals` button
- In the `Add a new variable` field, give the variable a name (e.g. _`new_global_number`_)
- In the `Inital Value` field, type `15` as the value, this will also populate the `Current Value` field
- Press the `Save` button
- To get back to the main window, press the `X` button

![Global Variables](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/09_globalAndDynamicVariables/Global_Variables.PNG)

We now have our first globally available variable set and it's ready to use, I've chosen a _number_ for a reason, it's to demonstrate something that can be a little bit confusing at first. This applies to both the `environment` and `global` variables but as we're looking at the globals at the moment, I'll show you here.

Everything you store as a variable is saved in the JSON environment file as a `string` so although we added the `number` 15, what we really added was the `string` "15". I'm going to show you a quick test so that you can see why this _could_ be confusing at first, once you know what's happening you will be able to amend your tests to look for the correct value.

The global variable can be used in the URL, Headers and the Request Body using the `{{..}}` syntax, just like the way we can use the environment level variables.

We are now going to use a different method to access the global variables value by using the `pm.globals.get('variableName')` syntax. As you start to type the variable name, an auto-complete box will be displayed with a list of options. You can use the `up` and `down` navigation arrows to move through the list and hit `Enter` on the one that you require.

This basic check is to confirm that the number 15 is equal to the value that we previously set in our variable. Looks quite straight forward...

```JavaScript
pm.test("15 should equal 15", () => pm.expect(15).to.equal(pm.globals.get('new_global_number')))
```

When the test is run, we see the following failure in the Response Test Results tab:

![Fifthteen](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/09_globalAndDynamicVariables/Fifthteen.PNG)

On the face of it, it looks fine and 15 does equal 15 but this fails because it's trying the assert that a number matches a string. What we need to do is very simple, we just need to parse our string as a number. We can use the [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) function and wrap this around the `pm.globals.get('new_global_number')` syntax.


```JavaScript
pm.test("15 should equal 15", () => pm.expect(15).to.equal(parseInt(pm.globals.get('new_global_number'))))
```

This time when the test is run, it will pass as it's performing the assertion against the _number_ 15. Something very simple but worth knowing as it _might_ save you time when debugging a failing test that looks like it should pass. :)

I've shown you how the variables can be set through the `Manage Environments` feature but these can also be set within either of the `Pre-Request Script` or `Tests` tabs.
We can do this by using the `pm.globals.set('variableName', 'variableValue')` syntax.

```JavaScript
pm.globals.set('another_global_number', 5555)
```

![Set Global Variable](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/09_globalAndDynamicVariables/Set_Global_Variable.gif)

As you can see in the clip, when the `Send` button is pressed, the global variable is saved. A shortcut way of seeing the variables that you have set is by using the `Environment quick look` - This can be opened using the `eye` icon in the top right of the application.

Global variables can be removed on an individual basis by using the `pm.globals.unset('variableName')` function, maybe you need to add some form of _teardown_ process so that you know that your tests are starting in the same state each time they run. Using a single `unset` is fine but you may have several variables that you need to clear, you could add a single line in the test to cover each one but this can get a bit messy. An alternative option is to write a quick function that iterates through a list of variables clearing each one. This might be an option if you want to clear _most_ but not _all_ the variables.      

```JavaScript
function cleanup() {
    const clean = ["variableName_1", "variableName_2"]
    for(let i = 0; i < clean.length; ++i){
        pm.environment.unset(clean[i])
    }
}
cleanup()
```

If you would like to clear **every** Global variable you have set in the application, you can use the `pm.globals.clear()` function in either of the `Pre-Request Script` or `Tests` tabs.

### Dynamic variables

Postman is full of different "helper" functions and the dynamic variables are absolutely one of these. They provide quick access to common pieces of data used in _some_ requests.

> "Postman has a few dynamic variables which you can use in your requests. Dynamic variables cannot be used in the Sandbox. You can only use them in the {{..}} format in the request URL / headers / body."

The variables in the list below can be added to Postman and these values will be populated at runtime (When you hit `Send`).

* `{{$timestamp}}` : Adds the current timestamp (Unix timestamp)
* `{{$guid}}`      : Adds a v4 style GUID
* `{{$randomInt}}` : Adds a random integer between 0 and 1000

These can be added to the request in the same way that we added the `environment` variables, in a previous example. For the dynamic variables, Postman uses the `auto-complete` feature to display the options available when using the `{{...}}` syntax. The clip below shows how these can be added to the URL:

![Add Dynamic Variable](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/09_globalAndDynamicVariables/Add_Dynamic_Variable.gif)

The same method can be used in the `Headers` tab of the request builder to add these dynamic variables:

![Dynamic Variable Headers](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/09_globalAndDynamicVariables/Dynamic_Variable_Headers.PNG)

Finally, these can be also be used in the `Request Body` when using one of the other HTTP methods like `POST`, we will cover this method in the next example.

```
{
	"totalprice" : {{$randomInt}}
}
```

As the extract from the documentation mentions, these can only be used in the `Request URL`, `Headers` and in the `Request Body` so this is not ideal if you want to use the same values within your `Pre-request Script` or `Tests` - Fear not, I will give you some pointers about how to achieve this using native JavaScript and a selection of the build-in modules that come with Postman.

There are lots of different modules that can be used within the `Pre-request Script` or `Tests` sandbox that are very helpful, over the series of the examples, I'll be using these more and more, so hopefully, these modules will become part of your own _Postman toolkit_ when you're using the application on your own projects.

The list of built-in modules can be found on the [Postman Sandbox API reference](https://www.getpostman.com/docs/postman/scripts/postman_sandbox_api_reference) page, this page provide links to the different modules to help you better understand what they can offer.

In the following pieces of code I'm going to be using the `console.log()` function - To see this output, you will need to open the `Postman Console` by selecting the icon in the bottom left of the application. Alternatively, you can use the shortcut command `CTRL + ALT + C` to open the console. This will open the console in a new window.

![Postman Console](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/09_globalAndDynamicVariables/Postman_Console.gif)

In the code below, I'm bringing in the [momentjs](http://momentjs.com/docs/), [uuid](https://www.npmjs.com/package/uuid) and the [Lodash](https://lodash.com/docs/3.10.1) modules using the `require('module_name')` syntax. The `require` statement is not needed to use the `3.10.1` version of Lodash but you would need to use the `require` statement for the newer version.

```JavaScript
// {{$timestamp}} alternative
const moment = require('moment')
console.log(`Timestamp: ${moment().valueOf()}`)

// {{$guid}} alternative
const uuid = require('uuid')
console.log(`Guid: ${uuid()}`)

// {{$randomInt}} alternative
const randomInt = _.random(1000)
console.log(`Random Number: ${randomInt}`)
```

We are using the `/ping` endpoint to make the request to help show this scenario. The code has been added to the `Pre-Request Script` tab, this sets the values **before** the request is made but this code could also be added to the `Tests` tab. This would set the values **after** the request has been made. An example of this can be seen in the clip below.

![Custom Dynamic Variables](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/09_globalAndDynamicVariables/Custom_Dynamic_Variables.gif)

That's the end of this example, we have covered how to create and clear `Global` variables and we have also taken a look at the `Dynamic` variables that Postman offers. In the next example we will be using the `/booking` route to `POST` data to the API and create some new bookings.

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
