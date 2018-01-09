# Creating our first test

In this example we will be taking our first look at the `Tests` feature. We will create a basic test to assert against some response data returned from the Restful-Booker API.

---

### Our first test

Making requests and building up collections is awesome but for me the real value that Postman provides, is in the `Tests` feature.

> "Test scripts are written in JavaScript, and are run after the response is received"

We're going to create a basic test using the `/booking/{id}` endpoint, we can use this as a starting point or introduction to get familiar with creating very quick tests - These tests will run each time we send a request and provide us with instant feedback.

We are not going to go too fast too quickly, there are so many different ways to use this feature and it can become overwhelming, we're going to stick to one of the _out of the box_ tests provided by Postman first and then get a little bit more advanced in the next example.

To access the feature all we need to do is select the `Tests` tab in the `Request Data Builder` section of the application.

![Access Tests Feature](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/07_creatingOurFirstTest/Access_Tests_Feature.gif)

As I mentioned before, Postman comes with a collection of _out of the box_ tests, this is a list of commonly used tests that assert against things like Response Status Codes, Response Timings, Response Headers, Checking that the Response Body contains certain values, etc. This section is very similar to the `Pre-request Script` Snippets but with a focus on _things_ that happen after the response is received rather than before the request is made.

![Test Scripts Snippets](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/07_creatingOurFirstTest/Test_Scripts_Snippets.PNG)

The most basic test, for me, is to check the [`Response Status Code`](https://httpstatuses.com/) is correct. There's no point in starting to assert against the data if we get back a non `2XX` status code from our `GET /booking/{id}` request. We can add this check to our request by selecting it from the `Snippets` list within the `Tests` section. The test template can be found towards the bottom of the list, so you will need to scroll down slightly.

Once selected, this places the code into the `Tests` sandbox.

![Response Code Test](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/07_creatingOurFirstTest/Response_Code_Test.gif)

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

This is one the test templates used by Postman, It's a simple JavaScript function that checks that the response status was `200`. Postman has created a pretty powerful set of [`pm.* global functions`](https://www.getpostman.com/docs/postman/scripts/postman_sandbox_api_reference), of which, `test` is one of them. The assertion text on the test is written in a more human readable format, which makes it easier to tell what the test is actually checking. We will be looking at this in more detail in later examples and create tests using more of the [Chai](http://chaijs.com/api/) assertion syntax.

__Note__ : I am absolutely **not** a JavaScript developer - The code in my examples will match my current experience. I'm always learning so if there is a cleaner way of writing the code please let me know. I'm open to all feedback :)

The templates are currently written in ES5 syntax but Postman also supports the [ES6](https://github.com/DrkSephy/es6-cheatsheet) syntax in the `Tests` sandbox. So the template code can be refactored slightly to remove some of the things that are not _really_ needed to run the test:

```javascript
pm.test('200 Status Code', () => pm.response.to.have.status(200))
```

You might see me using some ES6 syntax within my examples, you don't have to do that - It's just personal preference. For this first test example, I'm going to stick with the template that Postman offers and then modify those slightly when we move on to creating more advanced tests.

When any data is added to a section in the `Request Data Builder`, Postman shows a little `indicator` next to the tab header. That can either be a green dot or a number wrapped in brackets, that indicates how many `Headers` have been set. It's just something useful to know. :)

![Indicator](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/07_creatingOurFirstTest/Indicator.PNG)

### Running our first test

We now have the test code in place and we are ready to run this for the first time. All we need to do is press `Send` - Simple right?!

![First Test](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/07_creatingOurFirstTest/First_Test.gif)

The request is sent as normal and then the test code runs against the response. As we can see in the clip above, the feedback from the test can be seen in the `Response` section under the `Test Results` tab. The tab header displays an green `(1/1)` indicator, this tells us that the test we added to the request, has passed.

Let's take a quick look at what this would look like if it had failed...

![Failed Test](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/07_creatingOurFirstTest/Failed_Test.gif)

This time I changed the test to look for a `201` status code, we know that if the API is alive, this endpoint returns a `200` so we can cause the test to fail. In the `Test Results` section we can see that the tab header now displays a red `(0/1)` indicator. Postman offers us feedback to tell us why it _thinks_ the test failed:  

```javascript
Status code is 201 | AssertionError:expected response to have status code 201 but got 200
```

### Skipping a test

I wanted to share one last thing that I learnt while creating this example, I don't know _everything_ about the application so I'm really happy when I uncover new knowledge. Postman gives you the ability to `skip` a test, maybe you have a suite of tests that you want to run on a single request but you need to _jump over_ one of them. In the past, I would just comment out the test code so it wasn't included but you can now just use the `.skip` feature.

```javascript
pm.test.skip("Move over this test", function () {
    //This test is still being created
});

pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```

![Skipped Test](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/07_creatingOurFirstTest/Skipped_Test.gif)

You can see that it _runs_ all the tests but skips over the test with the `.skip`. In the `Test Results` we can see the `Skipped` label next to the test and also see this in the filtered test results. Personally, I don't think it should be included in the tab indicator. I could `.skip` both tests and it would still display a green `(2/2)` indicator, which for me, it's kind of a false report. Maybe an orange `(0/2)` would work better in that scenario to show that it hasn't really passed or failed anything.

That's the end of this example, we learnt how to create a basic test and check the feedback. In the next example we will be expanding on this and asserting against the response body data using the Chai assertion syntax.  

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
