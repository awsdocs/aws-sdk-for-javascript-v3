--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Create the Blog page in HTML<a name="kinesis-page-scrolling-create-html"></a>

The HTML for the blog page consists mainly of a series of paragraphs contained within a `<div>` element\. The scrollable height of this `<div>` is used to help calculate how far a reader has scrolled through the content as they read\. The HTML also contains a `<script>` element which adds the `main.js`\.This file contains the browser script that captures scroll progress on the page and reports it to Kinesis and the required AWS SDK for JavaScript modules\. You create this script using *webpack*, as described in the [Bundling the browser script](kinesis-page-scrolling-full.md) section of this example\.

```
<!DOCTYPE html>
<html>
<head>
    <title>AWS SDK for JavaScript (V3) - Amazon Kinesis Application</title>
</head>
<body>
<div id="BlogContent" style="width: 60%; height: 800px; overflow: auto;margin: auto; text-align: center;">
    <div>
        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum vitae nulla eget nisl bibendum feugiat. Fusce rhoncus felis at ultricies luctus. Vivamus fermentum cursus sem at interdum. Proin vel lobortis nulla. Aenean rutrum odio in tellus semper rhoncus. Nam eu felis ac augue dapibus laoreet vel in erat. Vivamus vitae mollis turpis. Integer sagittis dictum odio. Duis nec sapien diam. In imperdiet sem nec ante laoreet, vehicula facilisis sem placerat. Duis ut metus egestas, ullamcorper neque et, accumsan quam. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
        </p>
        <!-- Additional paragraphs in the blog page appear here -->
    </div>
</div>
<script src="main.js"></script>
</body>
</html>
```