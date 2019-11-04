# Requests With a Node\.js Stream Object<a name="requests-using-stream-objects"></a>

You can create a request that streams the returned data directly to a Node\.js `Stream` object by calling the `createReadStream` method on the request\. Calling `createReadStream` returns the raw HTTP stream managed by the request\. The raw data stream can then be piped into any Node\.js `Stream` object\.

This technique is useful for service calls that return raw data in their payload, such as calling `getObject` on an Amazon S3 service object to stream data directly into a file, as shown in this example\. 

```
const S3 = require('@aws-sdk/client-s3')
const s3Client = new S3.S3Client()
var params = {Bucket: 'myBucket', Key: 'myImageFile.jpg'};
var file = require('fs').createWriteStream('/path/to/file.jpg');
s3Client.send(new GetObjectCommand(params)).createReadStream().pipe(file);
```

When you stream data from a request using `createReadStream`, only the raw HTTP data is returned\. The SDK does not post\-process the data\.

Because Node\.js is unable to rewind most streams, if the request initially succeeds, then retry logic is disabled for the rest of the response\. In the event of a socket failure while streaming, the SDK won't attempt to retry or send more data to the stream\. Your application logic needs to identify such streaming failures and handle them\.

 Avoid using a callback to trap errors in the `getObject` call, such as when the key is not valid, as this tactic results in a race condition where two requests are opened at once, but the data from both are piped to the same destination\.

The solution is to attach an error event listener to the Amazon S3 stream, as shown in the following example\.

```
var fileStream = fs.createWriteStream('/path/to/file.jpg');
var s3Stream = s3Client.send(new GetObjectCommand(
  {Bucket: 'myBucket', Key: 'myImageFile.jpg'}
)).createReadStream();
// Listen for errors returned by the service
s3Stream.on('error', function(err) {
    // NoSuchKey: The specified key does not exist
    console.error(err);
});

s3Stream.pipe(fileStream).on('error', function(err) {
    // capture any errors that occur when writing data to the file
    console.error('File Stream:', err);
}).on('close', function() {
    console.log('Done.');
});
```