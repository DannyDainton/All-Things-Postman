# Extending our tests

For this example we will be taking the knowledge gained from creating our first basic test and extending this to cover more of the response data.

---

We will continue to use the `/booking/{id}` endpoint on the Restful-Booker API, this is perfect for what we need in order to further expand our knowledge of the `Tests` feature in Postman. Our first basic test was checking that we were getting back a `200` status code from our `GET` request. I explained how to do this one way but Postman offers you multiple different ways of checking this response. As you can see in the code snippet below, we are checking the same piece of response data but in four different ways. Which way you choose is entirely up to you, I just wanted to show you that there is always more than one way of doing something so explore the feature and find the method that works for you, in your context.

```javascript
// This checks the status code number
pm.test("Test 1: 200 Status Code", () => pm.response.to.have.status(200))

// This checks the status text string
pm.test('Test 2: 200 Status Code', () => pm.response.to.have.status('OK'))

// This is using a pre-defined rule offered by Postman
pm.test('Test 3: 200 Status Code', () => pm.response.to.be.ok)

// This is using a different rule offered by Postman (.code) and checking that it equals 200
pm.test('Test 4: 200 Status Code', () => pm.expect(pm.response.code).to.equal(200))
```

![Same Check](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/08_extendingOurTests/Same_Check.gif)

I mentioned in a previous example that Postman has created a set of [`pm.* global functions`](https://www.getpostman.com/docs/postman/scripts/postman_sandbox_api_reference), some of which can be seen in the clip above, we will be using many of these as we work through the examples but I wanted to list out a few options from the `pm.response.to.be.*` object.

> The properties inside the pm.response.to.be object allows you to easily assert a set of pre-defined rules

```java
pm.response.to.be.info - Checks 1XX status code
pm.response.to.be.success - Checks 2XX status code
pm.response.to.be.redirection - Checks 3XX status code
pm.response.to.be.clientError - Checks 4XX status code
pm.response.to.be.serverError - Checks 5XX
pm.response.to.be.error - Checks 4XX or 5XX
pm.response.to.be.ok - Status code must be 200
pm.response.to.be.accepted - Status code must be 202
pm.response.to.be.badRequest - Status code must be 400
pm.response.to.be.unauthorized - Status code must be 401
pm.response.to.be.forbidden - Status code 403
pm.response.to.be.notFound - Status code of response is checked to be 404
pm.response.to.be.rateLimited - Checks whether response status code is 429
```

To make things easier and help you to create more meaningful tests, Postman provides a bunch of `pre-defined` rules which target exactly what you want to assert against rather than stringing together something a little less robust. So if you want to check for a `404` use the `.notFound` rule or anything in the `3XX` range use the `.redirection` etc. The options are available for you to use in any request you want.

Similar to this set of helper rules, the application also offers other objects within the global functions that you can assert against in your tests - `pm.response.to.have.header(key:String, optionalValue:String)` and `pm.response.responseTime:Number` are ones that are very helpful. For example:

```javascript
pm.test('Content-Type header is correct', () => pm.response.to.have.header('Content-Type', 'application/json; charset=utf-8'))
```  

```javascript
pm.test('Response Time under 200 ms', () => pm.expect(pm.response.responseTime).to.be.below(200))
```

There are a lot of useful global functions so it's worth checking out the [documentation](https://www.getpostman.com/docs/postman/scripts/postman_sandbox_api_reference) and practicing using some of these in your requests. You will be safe in the knowledge that you won't damage the Restful-Booker API in any way, that's the reason why it was created - It's a perfect practice platform.

### Checking the booking data

Ok, we have looked at everything so far apart from the actual booking data returned. We'll create a test to quickly check that the correct types are being used for the values returned from the API. Looking at an sample of the data we can see that it's using a mixture of `strings`, `numbers` and `boolean` types - We can use Postman to check these and inform us if any of the known value types have been changed during the development of the API. No one likes nasty surprises like that.    

```json
{
    "firstname": "Jim",
    "lastname": "Jones",
    "totalprice": 308,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2016-12-26",
        "checkout": "2017-04-16"
    }
}
```

__Note__ : There is a template test for checking the response body data within the `snippets` collection in the `Tests` tab but for this example I will be using my own version.

In order to _access_ the response data we need to use another one of the handy global functions, this time we're using `pm.response.json()`, which internally, does a [`JSON.parse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) on the response body and allows us to reference the data in the JSON object.

I've assigned `pm.response.json()` to the `jsonData` variable. You don't _need_ to do this but it allows you to keep the tests a little bit more readable.

```javascript
var jsonData = pm.response.json()
```

Here's the quick test that I've created to check the value types of the response body data. We are using the `pm.expect()` function and using key words from the built-in [`Chai`](http://chaijs.com/api/bdd/) syntax to formulate the test.  

```javascript
pm.test("Response data format is correct", () => {
    var jsonData = pm.response.json()
    pm.expect(jsonData.firstname).to.be.a('string')
    pm.expect(jsonData.lastname).to.be.a('string')
    pm.expect(jsonData.totalprice).to.a('number')
    pm.expect(jsonData.depositpaid).to.be.a('boolean')
    pm.expect(jsonData.bookingdates.checkin).to.be.a('string')
    pm.expect(jsonData.bookingdates.checkin).to.match(/^\d{4}-\d{2}-\d{2}$/)
    pm.expect(jsonData.bookingdates.checkout).to.be.a('string')
    pm.expect(jsonData.bookingdates.checkout).to.match(/^\d{4}-\d{2}-\d{2}$/)
});
```
I'll extract a couple of these lines and explain the syntax in a little more detail:

```javascript
// Here we are using the jsonData variable to get the response data
// Then using dot notation to access the keys
// The `checkin` key is inside the `bookingdates` object so we need to go down 2 levels to get the value
// Finally, we are using a language chain to improve readability and assert that the value is a `string`
pm.expect(jsonData.bookingdates.checkin).to.be.a('string')

// This is very similar to the test above but we are using regular expression to check the format
// The regex string is asserting that the pattern is `4 digits`-`2 digits`-`2 digits`
// A great regex practice site can be found at https://regex101.com/#javascript
pm.expect(jsonData.bookingdates.checkin).to.match(/^\d{4}-\d{2}-\d{2}$/)
```

![Response Format Check](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/08_extendingOurTests/Response_Format_Check.gif)

In the clip above you can see that the check passed and the response data was how we expected it to be....but was it...You can see that there was an extra property in the response that wasn't on the original response sample above. The `additionalneeds` property is an optional value so some of the bookings will have this and some will not - How are we going to check for this...I'm going to extend the test we created, to include a check for this too.

For this we can to use a JavaScript [Conditional Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

![Conditional Operator](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/08_extendingOurTests/Conditional_Operator.PNG)

So what's it doing?! It's basically checking the response body data for the `additionalneeds` property, if it's missing this check would return an `undefined` value. In that case we would `skip` over the test but if the `additionalneeds` property is within the response body, then we check to see if the value is a `string`.

 ```javascript
 (pm.response.json().additionalneeds === undefined ? pm.test.skip : pm.test)('Customer has additional needs', () => {
         pm.expect(pm.response.json().additionalneeds).to.be.a('string')
 });
 ```

The first request was sent and the property was _missing_ from the response body so the test was `Skipped` - This was shown in the `Test Results` tab in the `Response` section.

![Property Missing](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/images/08_extendingOurTests/Property_Missing.PNG)

A second request was made to return data from another `bookingid` and this time the test was run and passed as the property was visible in the response body data and the type was a `string`.

![Additional Needs Property](https://github.com/DannyDainton/All-Things-Postman/blob/master/Public/gifs/08_extendingOurTests/Additional_Needs_Property.gif)

We can also use an `if/else` statement to get the same result - The syntax for this looks slightly different but it's basically doing the same thing when we send the request. Practice using different methods to find what works for you.

```javascript
if (pm.response.json().additionalneeds === undefined){
    pm.test.skip("Customer has no additional needs", () => {
        return
    })
} else {
    pm.test("Customer has additional needs", () => {
        pm.expect(pm.response.json().additionalneeds).to.be.a('string')
    })
}
```

We've come to the end of this example, there were a lot of different things happening here. Take your time and work through the content to get familiar with creating different types of tests. As always, if anything doesn't make sense or if you have any questions - Please get in touch!!

---
[Back to the Examples](https://github.com/DannyDainton/All-Things-Postman#example-guides)
