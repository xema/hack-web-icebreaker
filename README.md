# Hack: Icebreaker

Allows the participants of a presentation to go to the Icebreaker app and see Facebook Likes they have in common with others in the audience.

Authors: Matt Kelly (mattwkelly), Christine Abernathy (caabernathy)

## What's a "Hack"?

We love talking to developers. Rather than talk over boring slides, we like diving straight into code and show devs how to build a new app from scratch.

Most hacks are built for a specifically presentation and may not be updated afterward.  So it's important to note that THIS CODE MAY NOT BE MAINTAINED ANY MORE and could contain bugs.  We always accept pull requests if you spot anything, though!

## Installing

This section will walk you through the following:

* Getting Started
* Creating your Facebook app
* Setting up the the backend server using Heroku Cloud Services
* Setting up the the backend server using your own hosting
* Installing the web app
* Stepping through the demo

### Getting Started

Your install package should include the files containing the Icebreaker server side PHP files. This includes the completed and initial demo web app.

To get the sample code do the following:

* Install [Git](http://git-scm.com/).

* Pull the samples package from GitHub:

             git clone git://github.com/fbsamples/mobile-web-icebreaker

### Creating your Facebook app

First set up a Facebook app using the Developer app:

* Create a new [Facebook app](https://developers.facebook.com/apps)
* Enter the `App Namespace` when creating your app. You can choose a simple string to identify your app, such as ''icebreaker'', but it must be unique.

### Setting up the the backend server using Heroku Cloud Services

You can always use your own backend server and database to host the web app files. These instructions apply if you choose to use the Heroku Cloud Services.

* Go to your app on the Facebook [Dev app](https://developers.facebook.com/apps)

* Click on Edit settings

* In the _Hosting URL_ setting, click on the _Get one_ link. Select Heroku as the provider. When asked choose a PHP environment.

* Follow the instructions for setting up your server-side. Note the Cloud Services Hosting URL that is generated; you will enter this in a later step.

* Edit your [Dev app](https://developers.facebook.com/apps) settings to add the Heroku information:
  * Under Basic settings modify _App Domain_ to add ''herokuapp.com''

* Ensure that you have installed the [Heroku Toolbelt](http://devcenter.heroku.com/articles/facebook#heroku_account_and_tools_setup) and followed the setup instructions in the email.

* Fetch your [server app code](http://devcenter.heroku.com/articles/facebook#editing_your_app)

* Create a [ClearDB database](http://www.cleardb.com/developers/connect/paas/heroku) that your app will use. Note that you will need a verified Heroku account to set up a database. To verify your account you will need to provide Heroku with a credit card. You can follow the same instructions listed to connect to ClearDB from Java if there are no instructions specific to PHP. When you create the database, you should note the `CLEARDB_DATABASE_URL` value returned that will be in the form:

        mysql://username:password@server/database_name

* Add the web app server-side PHP files to your Heroku app:
  * Create a sub-directory in your Heroku local app's main directory called ''icebreaker''
  * Copy the mobile-web-icebreaker/* files to your Heroku local app's ''icebreaker'' directory
  * For the files index_complete.html and api.php wherever you find them, replace:
     * `YOUR_APP_ID` with your app ID
     * `YOUR_APP_SECRET` with your app secret
     * `YOUR_HOST_URL` with your Heroku hosting URL
  * Modify db.php and replace:
     * `YOUR_DB_USERNAME` with your database's username
     * `YOUR_DB_PASSWORD` with your database's password
     * `YOUR_DB_SERVER` with your database's host name
     * `YOUR_DB_NAME` with your database name
  * Commit and push the local additions up to Heroku
    * git add .
    * git commit -am "icebreaker"
    * git push heroku master

### Setting up the the backend server using your own hosting

You can run the sample web app if you have hosting that includes a MySQL server. If you follow the steps below you do not need to set up Heroku.

* Edit your [Dev app](https://developers.facebook.com/apps) settings to add your app domain information:
  * Under Basic settings modify _App Domain_ to add your domain, example ''example.com''

* Create a MySQL database and note the database username, password, server, and database name

* Configure the web app files you retrieved from GitHub:
  * Create a sub-directory in your web site's main directory called ''icebreaker''
  * Copy the mobile-web-icebreaker/* files to your app's ''icebreaker'' directory
  * For the files index_complete.html and api.php wherever you find them, replace:
     * `YOUR_APP_ID` with your app ID
     * `YOUR_APP_SECRET` with your app secret
     * `YOUR_HOST_URL` with your hosting URL
  * Modify db.php and replace:
     * `YOUR_DB_USERNAME` with your database's username
     * `YOUR_DB_PASSWORD` with your database's password
     * `YOUR_DB_SERVER` with your database's host name
     * `YOUR_DB_NAME` with your database name


### Installing the web app

**Installing the database tables**

* Go to `YOUR_HOST_URL`/icebreaker/manage_db_table.html
* Click on the ''Create Table'' link to create the table that will be used in the demo

Note: If you want to check the completed demo that is part of the package, then overwrite the index.html file with index_complete.html. Be sure to make a backup of the initial index.html if you still wish to step through the demo.

### Stepping through the demo

**Notes: Checking in code**

During the demo steps we may be checking in code modifications if we use Heroku. To do we will run the following commands after each step:

        git commit -a -m "checkpoint"
        git push heroku master

You need to follow the above steps if you are hosting your backend code on Heroku.


**Step 1: Set the Viewport**

The text is too small when viewed on a mobile device. Set the viewport by adding the code below in the &lt;head> tag.

        <meta name="viewport" content="initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />


**Step 2: Set up Authentication**

* Edit your [Dev app](https://developers.facebook.com/apps) settings to configure the app links:
  * Under Website settings set the _Site URL_ to `YOUR_HOST_URL`/icebreaker/
  * Under Mobile Web settings set the _Mobile Web URL_ to `YOUR_HOST_URL`/icebreaker/

* Set up authentication referral for your app
  * Under Settings > Auth Dialog, go to the Authenticated Referrals section
  * In the _User & Friend Permissions_ add ''user_likes''

* Include the Facebook JavaScript SDK, instantiate the Facebook instance, and monitor authorization status changes. Add the following code inside the &lt;body> tag

        <div id="fb-root"></div>

        <script>
        (function() {
          var e = document.createElement('script'); e.async = true;
          e.src = document.location.protocol + '//connect.facebook.net/en_US/all.js';
          document.getElementById('fb-root').appendChild(e);
          }())
        window.fbAsyncInit = function() {
          FB.init({ appId: 'YOUR_APP_ID',
          status: true,
          cookie: true,
          xfbml: true,
          oauth: true})
          FB.Event.subscribe('auth.statusChange', handleStatusChange);
        };

        function handleStatusChange(response) {
          console.log(response);
        }

        </script>


  Replace _YOUR_APP_ID_ with your app ID.

* Add a link to allow the user to log in.

          <div id="user-info">Hello<br>
              <a href="#" onclick="login();">Login</a><br>
          </div>

* Add a JavaScript function to handle login clicks.

          function login() {
            FB.login(function(response) { }, {scope:'user_likes'});
          }


**Step 3: Personalization**

When the user is authenticated personalize the user interface by displaying their name and profile picture.


* Modify the function that handles auth status changes, make a call to a new method that will show some personal information.

          function handleStatusChange(response) {
            if (response.authResponse) {
              console.log(response);
              updateUserInfo(response);
            }
          }

          function updateUserInfo(response) {
            FB.api('/me&fields=likes,id,name', function(response) {
              document.getElementById('user-info').innerHTML = '<img src="https://graph.facebook.com/' + response.id + '/picture">' + response.name;
            });
          }

**Step 4: Add Social Design**

Add code to get a user's common likes and compare them against likes from users that are stored in a database.

* Add the following code to the &lt;head> tag

          <script src="jquery-1.5.1.min.js"></script>

* Add the following code to the in the auth response handler

          getCommonLikes(response);

* Add the following code in the script tag in the &lt;body> tag

        function getCommonLikes(response) {
            var output = '';

            $(document).ready(function () {
              $.ajax({url: 'api.php', type: "POST", dataType: 'json', data: { method: "getCommonLikes", location: location.coords }, success: function(data) {
              output = '';

              console.log(data);

              for (var i = 0; i < data.likes.length; i++) {
                output += '<h3>' + data.likes[i].like_name + '</h3>';

                if (data.likes[i].uids) {
                  output += '<h4>Shared with</h4><div id="user-friends">';
                  for (var n = 0; n < data.likes[i].uids.length; n++) {
                    output += '<img src="http://graph.facebook.com/' + data.likes[i].uids[n].id + '/picture"> ' + data.likes[i].uids[n].name;
                  }

                  output += '</div><br><br>';
                }
              }

              $("body").append(output);
              console.log(output);
            }});
          });
        }

**Step 5: Add Social Channels: News Feed, Requests, Social Plugins**


* Add the following code to the script tag in the &lt;body> tag

        function publishStory() {
            FB.ui({
                method: 'feed',
                name: 'I\'m building a social mobile web app!',
                caption: 'This web app is going to be awesome.',
                description: 'Check out Facebook\'s developer site to start building.',
                link: 'https://morning-day-3487.herokuapp.com/icebreaker/',
                picture: 'http://www.facebookmobileweb.com/hackbook/img/facebook_icon_large.png'
            },
            function(response) {
                console.log('publishStory response: ', response);
            });
            return false;
        }

        function sendRequest() {
            FB.ui({
                method: 'apprequests',
                message: 'Check out this awesome app!'
            },
            function(response) {
                console.log('sendRequest response: ', response);
            });
        }



* Add the following code before the end of the &lt;body> tag

        <br>
            <a href="#" onclick="publishStory();">Publish feed story</a><br><br>
        <a href="#" onclick="sendRequest();">Send request</a><br><br>
        <div id="social-plugin">
            <fb:like width="250"></fb:like>
        </div>
        <fb:comments href="https://morning-day-3487.herokuapp.com/icebreaker/" num_posts="2" width="300"></fb:comments>

## Contributing

All contributors must agree to and sign the [Facebook CLA](https://developers.facebook.com/opensource/cla) prior to submitting Pull Requests. We cannot accept Pull Requests until this document is signed and submitted.

## License

Copyright 2012-present Facebook, Inc.

You are hereby granted a non-exclusive, worldwide, royalty-free license to use, copy, modify, and distribute this software in source code or binary form for use in connection with the web services and APIs provided by Facebook.

As with any software that integrates with the Facebook platform, your use of this software is subject to the Facebook Developer Principles and Policies [http://developers.facebook.com/policy/]. This copyright notice shall be included in all copies or substantial portions of the software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.