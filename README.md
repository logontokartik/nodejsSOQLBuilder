What is Node.js
---------------

Check out the video [http://vimeo.com/87925925], there are many other great tutorials on the web if you want to dive deep.

Getting Started
---------------
1.	Install Node.js from http://nodejs.org/download/
2.	After the install is successful invoke the interactive node.js shell, in order to get out press CTRL + C
```sh
$ node
>console.log(“Hello Salesforce”)
Hello Salesforce
```
3.	Writing a node.js program is as simple as creating a new file with a '.js' extension. For example you could create a simple 'hello_salesforce.js' file with the following content:
```sh
var http = require('http');  
var server = http.createServer(function(req, res) {   
             res.writeHead(200);   
             res.end('Hello Salesforce'); 
            }); server.listen(3000);
```
4.	Lets run the file by typing the following in terminal
```sh
$ node hello_salesforce.js
```
5.	Now just navigate to [http://localhost:3000] and you can see Hello Salesforce printed on your browser

Using Node.js and Salesforce ForceTK API
---------------------------------------

> *Why Node.js?*

Because of the browser’s cross-origin restrictions, your JavaScript application hosted on your own server (or localhost) will not be able to make API calls directly to the *.salesforce.com domain. The solution is to proxy your API calls through your own server. Using Node, Express & Request its very easy to create a proxy server

```sh
var express = require('express'),
    http = require('http'),
    request = require('request'),
    app = express();
 
app.use(express.static(__dirname + '/client'));
 
app.all('/proxy', function(req, res) {
    var url = req.header('SalesforceProxy-Endpoint');
    console.log("proxying: " + url);
    request({ url: url, 
              method: req.method, 
              json: req.body, 
              headers: {'Authorization': req.header('X-Authorization')}})
        .pipe(res);
});
 
app.set('port', process.env.PORT || 3000);
 
app.listen(app.get('port'), function () {
    console.log('Express server listening on port ' + app.get('port'));
});

```

Installing and running app locally
----

1.	Download the code here and unzip the file anywhere on your file system, or clone this repository.
2.	Login to your development environment.
You can sign up for a free developer environment here if you don’t already have one.
3.	Create a Connected App (Left Nav > Build > Create > Apps > Connected Apps > New) defined as follows:
![Image](http://coenraets.org/blog/wp-content/uploads/2014/02/ConnectedApp.jpg)
4.	After saving your Connected App, copy the consumer key to your clipboard.
5.	Open the force-api-mini-explorer/js/app.js file and paste the consumer key as the value forclientId on line 2.
6.	Open a command prompt (terminal), cd to the force-mini-api-explorer folder, and type the following to install the Node.js modules required by the proxy server (express and require):
```sh
npm install
```
7. Start the server
```sh
node server
```
8.	Open a browser and access the following url: http://localhost:3000.
9.	Click the Login button and authenticate.
10.	Type a SOQL query and click the Run Query button

Deploying your application to Heroku
-----------
1.	If you already don’t have a Heroku Account, go ahead and create on at heroku.com
2.	Follow the steps in [https://devcenter.heroku.com/articles/quickstart] after you successfully created a developer account, please make sure you install heroku-toolbelt
3.	Now that you have setup heroku on your machine, you need to add your repo to git and deploy to heroku
4.	Navigate to home of the directory where your app lives and enter following
```sh
git init
git add .
git commit –m “Salesforce Heroku App”
```
5. Create Heroku App
```sh
heroku create
```
6.	Take note of the application name that Heroku gave to your app. For example, murmuring-waters-3927. Use the actual name of your app in the instructions below.
7.	In Salesforce, create a Connected App as described in the local instructions above. For the Callback URL, make sure you use https and your Heroku app name. For example:https://murmuring-waters-3927.herokuapp.com/oauthcallback.html
8.	Modify app.js and specify your connected app clientId and connection URLs matching your Heroku app name. For example:
a.	redirectURI: https://murmuring-waters-3927.herokuapp.com/oauthcallback.html
b.	proxyURL: For example: https://murmuring-waters-3927.herokuapp.com/proxy

9.	Commit the changes and deploy to heroku
```sh
git add . -–all
git commit –m “Changed app parameters”
git push heroku master
```
10.	Run your application
```sh
heroku open
```
11.	Click the Login button (upper right corner) and enter your Salesforce credentials
12.	Click the Run Query Button

Note: In case you get fatal error permission denied, try the below [https://devcenter.heroku.com/articles/keys]
```sh
ssh-keygen -t rsa
heroku keys:add
```
** Thanks **

* [Christophe Coenraets] http://coenraets.org/
* [Harshit Pandey] http://www.oyecode.com/
* [node.js] http://nodejs.org
* [Twitter Bootstrap] http://twitter.github.com/bootstrap/
* [jQuery] http://jquery.com
* [express] http://expressjs.com
