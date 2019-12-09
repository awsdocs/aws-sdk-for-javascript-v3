# Viewing Photos in an Amazon S3 Bucket from a Browser<a name="s3-example-photos-view"></a>

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This browser script code example shows:**
+ How to create a photo album in an Amazon Simple Storage Service \(Amazon S3\) bucket and allow unauthenticated users to view the photos\.

## The Scenario<a name="s3-example-photos-view-scenario"></a>

In this example, a simple HTML page provides a browser\-based application for viewing the photos in a photo album\. The photo album is in an Amazon S3 bucket into which photos are uploaded\.

![\[JavaScript in a browser script using Amazon S3 buckets for photo albums.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/s3-photo-album-example.png)

The browser script uses the SDK for JavaScript to interact with an Amazon S3 bucket\. The script uses the [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property) method of the Amazon S3 client class to enable you to view the photo albums\.

## Prerequisite Tasks<a name="s3-example-photos-view-scenario-prerequisites"></a>

To set up and run this example, first complete these tasks\.

**Note**  
In this example, you must use the same AWS Region for both the Amazon S3 bucket and the Amazon Cognito identity pool\.

### Create the Bucket<a name="s3-example-photos-view-prereq-bucket"></a>

In the [Amazon S3 console](https://console.aws.amazon.com/s3/), create an Amazon S3 bucket where you can store albums and photos\. For more information about using the console to create an S3 bucket, see [Creating a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\.

As you create the S3 bucket, be sure to do the following:
+ Make note of the bucket name so you can use it in a subsequent prerequisite task, Configure Role Permissions\.
+ Choose an AWS Region to create the bucket in\. This must be the same Region that you'll use to create an Amazon Cognito identity pool in a subsequent prerequisite task, Create an Identity Pool\.
+ In the **Create Bucket** wizard, on the **Public access settings\.\.\.** page, in the **Manage public access control lists \(ACLs\)** group, clear these boxes: **Block new public ACLs and uploading public objects** and **Remove public access granted through public ACLs**\.

  For information about how to check and configure bucket permissions, see [Setting Bucket and Object Access Permissions](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/set-permissions.html) in the *Amazon Simple Storage Service Console User Guide*\.

### Create an Identity Pool<a name="s3-example-photos-view-prereq-idpool"></a>

In the [Amazon Cognito console](https://console.aws.amazon.com/cognito/), create an Amazon Cognito identity pool, as described in [Step 1: Create an Amazon Cognito Identity Pool](getting-started-browser.md#getting-started-browser-create-identity-pool) of the *Getting Started in a Browser Script* topic\.

As you create the identity pool:
+ Make note of the identity pool name, as well as the role name for the **unauthenticated** identity\.
+ On the **Sample Code** page, select "JavaScript" from the **Platform** list\. Then copy or write down the sample code\.
**Note**  
You must choose "JavaScript" from the **Platform** list for your code to work\.

### Configure Role Permissions<a name="s3-example-photos-view-prereq-perms"></a>

To allow viewing of albums and photos, you have to add permissions to an IAM role of the identity pool that you just created\. Start by creating a policy as follows\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Policies**, and then choose the **Create policy** button\.

1. On the **JSON** tab, enter the following JSON definition, but replace *BUCKET\_NAME* with the name of the bucket\.

   ```
   {
      "Version": "2012-10-17",
      "Statement": [
         {
            "Effect": "Allow",
            "Action": [
               "s3:ListBucket"
            ],
            "Resource": [
               "arn:aws:s3:::BUCKET_NAME"
            ]
         }
      ]
   }
   ```

1. Choose the **Review policy** button, name the policy and provide a description \(if you want\), and then choose the **Create policy** button\.

   Be sure to make note of the name so that you can find it and attach it to the IAM role later\.

After the policy is created, navigate back to the [IAM console](https://console.aws.amazon.com/iam/)\. Find the IAM role for the **unauthenticated** identity that Amazon Cognito created in the previous prerequisite task, Create an Identity Pool\. You use the policy you just created to add permissions to this identity\.

Although the workflow for this task is generally the same as [Step 2: Add a Policy to the Created IAM Role](getting-started-browser.md#getting-started-browser-iam-role) of the *Getting Started in a Browser Script* topic, there are a few differences to note:
+ Use the new policy that you just created, not a policy for Amazon Polly\.
+ On the **Attach Permissions** page, to quickly find the new policy, open the **Filter policies** list and choose **Customer managed**\.

For additional information about creating an IAM role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

### Configure CORS<a name="s3-example-photos-view-cors-configuration"></a>

Before the browser script can access the Amazon S3 bucket, you have to set up its [CORS configuration](browser-js-considerations.md#configuring-cors-s3-bucket) as follows\.

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <AllowedMethod>HEAD</AllowedMethod>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```

### Create Albums and Upload Photos<a name="s3-example-photos-view-create-albums"></a>

Because this example only allows users to view the photos that are already in the bucket, you need to create some albums in the bucket and upload photos to them\.

**Note**  
For this example, the file names of the photo files must start with a single underscore \("\_"\)\. This character is important later for filtering\. In addition, be sure to respect the copyrights of the owners of the photos\.

1. In the [Amazon S3 console](https://console.aws.amazon.com/s3/), open the bucket that you created earlier\.

1. On the **Overview** tab, choose the **Create folder** button to create folders\. For this example, name the folders "album1", "album2", and "album3"\.

1. For **album1** and then **album2**, select the folder and then upload photos to it as follows:

   1. Choose the **Upload** button\.

   1. Drag or choose the photo files you want to use, and then choose **Next**\.

   1. Under **Manage public permissions**, choose **Grant public read access to this object\(s\)**\.

   1. Choose the **Upload** button \(in the lower\-left corner\)\.

1. Leave **album3** empty\.

## Defining the Webpage<a name="s3-example-photos-view-html"></a>

The HTML for the photo\-viewing application consists of a `<div>` element in which the browser script creates the viewing interface\. The first `<script>` element adds the SDK to the browser script\. The second `<script>` element adds the external JavaScript file that holds the browser script code\. 

For this example, the file is named `PhotoViewer.js`, and is located in the same folder as the HTML file\. To find the current SDK\_VERSION\_NUMBER, see the API Reference for the SDK for JavaScript at [https://docs\.aws\.amazon\.com/AWSJavaScriptSDK/latest/index\.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/)\.

```
<!DOCTYPE html>
<html>
  <head>
    <!-- **DO THIS**: -->
    <!--   Replace SDK_VERSION_NUMBER with the current SDK version number -->
    <script src="https://sdk.amazonaws.com/js/aws-sdk-SDK_VERSION_NUMBER.js"></script>
    <script src="./PhotoViewer.js"></script>
    <script>listAlbums();</script>
  </head>
  <body>
    <h1>Photo Album Viewer</h1>
    <div id="viewer" />
  </body>
</html>
```

## Configuring the SDK<a name="s3-example-photos-view-config"></a>

Obtain the credentials you need to configure the SDK by calling the `CognitoIdentityCredentials` method\. You need to provide the Amazon Cognito identity pool ID\. Then create an `AWS.S3` service object\.

```
// **DO THIS**:
//   Replace BUCKET_NAME with the bucket name.
//
var albumBucketName = 'BUCKET_NAME';

// **DO THIS**:
//   Replace this block of code with the sample code located at:
//   Cognito -- Manage Identity Pools -- [identity_pool_name] -- Sample Code -- JavaScript
//
// Initialize the Amazon Cognito credentials provider
AWS.config.region = 'REGION'; // Region
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
    IdentityPoolId: 'IDENTITY_POOL_ID',
});

// Create a new service object
var s3 = new AWS.S3({
  apiVersion: '2006-03-01',
  params: {Bucket: albumBucketName}
});

// A utility function to create HTML.
function getHtml(template) {
  return template.join('\n');
}
```

The rest of the code in this example defines the following functions to gather and present information about the albums and photos in the bucket\.
+ `listAlbums`
+ `viewAlbum`

## Listing Albums in the Bucket<a name="s3-example-photos-view-list-albums"></a>

To list all of the existing albums in the bucket, the application's `listAlbums` function calls the `listObjects` method of the `AWS.S3` service object\. The function uses the `CommonPrefixes` property so that the call returns only objects that are used as albums \(that is, the folders\)\.

The rest of the function takes the list of albums from the Amazon S3 bucket and generates the HTML needed to display the album list on the webpage\.

```
// List the photo albums that exist in the bucket.
function listAlbums() {
  s3.listObjects({Delimiter: '/'}, function(err, data) {
    if (err) {
      return alert('There was an error listing your albums: ' + err.message);
    } else {
      var albums = data.CommonPrefixes.map(function(commonPrefix) {
        var prefix = commonPrefix.Prefix;
        var albumName = decodeURIComponent(prefix.replace('/', ''));
        return getHtml([
          '<li>',
            '<button style="margin:5px;" onclick="viewAlbum(\'' + albumName + '\')">',
              albumName,
            '</button>',
          '</li>'
        ]);
      });
      var message = albums.length ?
        getHtml([
          '<p>Click on an album name to view it.</p>',
        ]) :
        '<p>You do not have any albums. Please Create album.';
      var htmlTemplate = [
        '<h2>Albums</h2>',
        message,
        '<ul>',
          getHtml(albums),
        '</ul>',
      ]
      document.getElementById('viewer').innerHTML = getHtml(htmlTemplate);
    }
  });
}
```

## Viewing an Album<a name="s3-example-photos-view-viewing-album"></a>

To display the contents of an album in the Amazon S3 bucket, the application's `viewAlbum` function takes an album name and creates the Amazon S3 key for that album\. The function then calls the `listObjects` method of the `AWS.S3` service object to obtain a list of all the objects \(the photos\) in the album\.

The rest of the function takes the list of objects that are in the album and generates the HTML needed to display the photos on the webpage\.

```
// Show the photos that exist in an album.
function viewAlbum(albumName) {
  var albumPhotosKey = encodeURIComponent(albumName) + '/_';
  s3.listObjects({Prefix: albumPhotosKey}, function(err, data) {
    if (err) {
      return alert('There was an error viewing your album: ' + err.message);
    }
    // 'this' references the AWS.Response instance that represents the response
    var href = this.request.httpRequest.endpoint.href;
    var bucketUrl = href + albumBucketName + '/';

    var photos = data.Contents.map(function(photo) {
      var photoKey = photo.Key;
      var photoUrl = bucketUrl + encodeURIComponent(photoKey);
      return getHtml([
        '<span>',
          '<div>',
            '<br/>',
            '<img style="width:128px;height:128px;" src="' + photoUrl + '"/>',
          '</div>',
          '<div>',
            '<span>',
              photoKey.replace(albumPhotosKey, ''),
            '</span>',
          '</div>',
        '</span>',
      ]);
    });
    var message = photos.length ?
      '<p>The following photos are present.</p>' :
      '<p>There are no photos in this album.</p>';
    var htmlTemplate = [
      '<div>',
        '<button onclick="listAlbums()">',
          'Back To Albums',
        '</button>',
      '</div>',
      '<h2>',
        'Album: ' + albumName,
      '</h2>',
      message,
      '<div>',
        getHtml(photos),
      '</div>',
      '<h2>',
        'End of Album: ' + albumName,
      '</h2>',
      '<div>',
        '<button onclick="listAlbums()">',
          'Back To Albums',
        '</button>',
      '</div>',
    ]
    document.getElementById('viewer').innerHTML = getHtml(htmlTemplate);
  });
}
```