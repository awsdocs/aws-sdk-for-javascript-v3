--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using an anonymous callback function<a name="using-a-callback-function"></a>

Each service object method can accept an anonymous callback function as the last parameter\. The signature of this callback function is as follows\.

```
function(error, data) {
    // callback handling code
};
```

This callback function executes when either a successful response or error data returns\. If the method call succeeds, the contents of the response are available to the callback function in the `data` parameter\. If the call doesn't succeed, the details about the failure are provided in the `error` parameter\.

Typically the code inside the callback function tests for an error, which it processes if one is returned\. If an error is not returned, the code then retrieves the data in the response from the `data` parameter\. The basic form of the callback function looks like this example\.

```
function(error, data) {
    if (error) {
        // error handling code
        console.log(error);
    } else {
        // data handling code
        console.log(data);
    }
};
```

In the previous example, the details of either the error or the returned data are logged to the console\. Here is an example that shows a callback function passed as part of calling a method on a service object\.

```
ec2.describeInstances(function(error, data) {
  if (error) {
    console.log(error); // an error occurred
  } else {
    console.log(data); // request succeeded
  }
});
```