# Web Federated Identity Examples<a name="config-web-identity-examples"></a>

Here are a few examples of using web federated identity to obtain credentials in browser JavaScript\. These examples must be run from an `http://` or `https://` host scheme to ensure the identity provider can redirect to your application\. 

## Login with Amazon Example<a name="config-web-identity-amazon-login-example"></a>

The following code shows how to use Login with Amazon as an identity provider\. 

```
<a href="#" id="login">
  <img border="0" alt="Login with Amazon"
    src="https://images-na.ssl-images-amazon.com/images/G/01/lwa/btnLWA_gold_156x32.png"
    width="156" height="32" />
</a>
<div id="amazon-root"></div>
<script type="text/javascript">
  var s3 = null;
  var clientId = 'amzn1.application-oa2-client.1234567890abcdef'; // client ID
  var roleArn = 'arn:aws:iam::AWS_ACCOUNT_ID:role/WEB_IDENTITY_ROLE_NAME';

  window.onAmazonLoginReady = function() {
    amazon.Login.setClientId(clientId); // set client ID

    document.getElementById('login').onclick = function() {
      amazon.Login.authorize({scope: 'profile'}, function(response) {
        if (!response.error) { // logged in
          credentials = new AWS.WebIdentityCredentials({
            RoleArn: roleArn,
            ProviderId: 'www.amazon.com',
            WebIdentityToken: response.access_token
          });

          s3 = new AWS.S3({ credentials: credentials });

          console.log('You are now logged in.');
        } else {
          console.log('There was a problem logging you in.');
        }
      });
    };
  };

  (function(d) {
    var a = d.createElement('script'); a.type = 'text/javascript';
    a.async = true; a.id = 'amazon-login-sdk';
    a.src = 'https://api-cdn.amazon.com/sdk/login1.js';
    d.getElementById('amazon-root').appendChild(a);
  })(document);
</script>
```

## Facebook Login Example<a name="config-web-identity-facebook-login-example"></a>

The following code shows how to use Facebook Login as an identity provider\.

```
<button id="login">Login</button>
<div id="fb-root"></div>
<script type="text/javascript">
var s3 = null;
var appId = '1234567890'; // Facebook app ID
var roleArn = 'arn:aws:iam::AWS_ACCOUNT_ID:role/WEB_IDENTITY_ROLE_NAME';

window.fbAsyncInit = function() {
  var s3 = null;
 
  // init the FB JS SDK
  FB.init({appId: appId});

  document.getElementById('login').onclick = function() {
    FB.login(function (response) {
      if (response.authResponse) { // logged in
        credentials = new AWS.WebIdentityCredentials({
            RoleArn: roleArn,
            ProviderId: 'graph.facebook.com',
            WebIdentityToken: response.authResponse.accessToken
          });

          s3 = S3.S3Client({ credentials: credentials })

        console.log('You are now logged in.');
      } else {
        console.log('There was a problem logging you in.');
      }
    });
  };
};

// Load the FB JS SDK asynchronously
(function(d, s, id){
   var js, fjs = d.getElementsByTagName(s)[0];
   if (d.getElementById(id)) {return;}
   js = d.createElement(s); js.id = id;
   js.src = "//connect.facebook.net/en_US/all.js";
   fjs.parentNode.insertBefore(js, fjs);
 }(document, 'script', 'facebook-jssdk'));
</script>
```

## Google\+ Sign\-in Example<a name="config-web-identity-google-login-example"></a>

The following code shows how to use Google\+ Sign\-in as an identity provider\. Unlike other providers, the access token used for web identity federation from Google is stored in `response.id_token` instead of `access_token`\. 

```
<span
  id="login"
  class="g-signin"
  data-height="short"
  data-callback="loginToGoogle"
  data-cookiepolicy="single_host_origin"
  data-requestvisibleactions="http://schemas.google.com/AddActivity"
  data-scope="https://www.googleapis.com/auth/plus.login">
</span>
<script type="text/javascript">
  var s3 = null;
  var clientID = '1234567890.apps.googleusercontent.com'; // Google client ID
  var roleArn = 'arn:aws:iam::AWS_ACCOUNT_ID:role/WEB_IDENTITY_ROLE_NAME';

  document.getElementById('login').setAttribute('data-clientid', clientID);
  function loginToGoogle(response) {
    if (!response.error) {
      credentials = new AWS.WebIdentityCredentials({
        RoleArn: roleArn, 
        WebIdentityToken: response.id_token
      });

      s3 = new AWS.S3({ credentials: Credentials })

      console.log('You are now logged in.');
    } else {
      console.log('There was a problem logging you in.');
    }
  }

  (function() {
    var po = document.createElement('script'); po.type = 'text/javascript'; po.async = true;
    po.src = 'https://apis.google.com/js/client:plusone.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(po, s);
  })();
 </script>
```