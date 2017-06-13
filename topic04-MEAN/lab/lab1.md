# Create basic Node.js application

## Lab Goal:
- Set up the project base and folder structure.
- Set up a base server which you can run.


## Project Setup
Clone the project repo: https://github.com/JameelB/node-todo

```
git clone https://github.com/JameelB/node-todo
```
or
```
git clone git@github.com:JameelB/node-todo.git
```

### 1. Create Folder Structure
Create the following folders and files as listed below:
```
- public        <!-- Contains all the files for the frontend angular application -->
  > core.js     <!-- Contains all of the angular code for the app -->
  > index.html  <!-- Main view -->
- package.json  <!-- Contains the NPM configuration to install dependencies/modules -->
- server.js     <!-- Node configuration -->
```
This is a simplified file structure for this tutorial. In larger application, this would be broken down
further to separate duties between client and server. Checkout [MEAN.io](http://mean.io/) which is a good boilerplate
to see best practices and how to separate file structure.

To see expected results for this step checkout the branch step-01: `git checkout step-01`

### 2. Configure package.json
The package.json holds configuration for the app. Node's package manager (NPM) will use this
to install any dependencies or modules that are specified here.

Copy the following below to your package.json:
```javascript
{
  "name"         : "node-todo",
  "version"      : "0.0.1",
  "description"  : "Simple todo application.",
  "main"         : "server.js",
  "author"       : "ScotchIO",
  "dependencies" : {
    "express"    : "~4.7.2",
    "mongoose"   : "~3.6.2",
    "morgan"     : "~1.2.2",
    "body-parser": "~1.5.2",
    "method-override": "~2.1.2"
    }
}
```
Save the file and run `npm install`. This will install the dependencies specified in your package.json

To see expected results for this step checkout the branch step-02: `git checkout step-02`

### 3. Node Configuration
In the `package.json` file, it specified that the main file would be `server.js`. This is the main
file for our Node app and where we will configure the entire application.

In this file the following configuration needs to be done:
- Database connection
- Mongoose models creation
- RESTful API routes definition
- Frontend Angular routes definition
- Set a port for the app to listen to.

```javascript
// server.js

    // set up ========================
    var express  = require('express');
    var app      = express();                        // create our app w/ express
    var mongoose = require('mongoose');              // mongoose for mongodb
    var morgan = require('morgan');                  // log requests to the console (express4)
    var bodyParser = require('body-parser');         // pull information from HTML POST (express4)
    var methodOverride = require('method-override'); // simulate DELETE and PUT (express4)

    // configuration =================

    var localDbUrl = 'mongodb://localhost/meanstacktutorial'; //set your local database URL
    mongoose.connect(localDbUrl); //Connects to your local database.

    app.use(express.static(__dirname + '/public'));                 // set the static files location /public/img will be /img for users
    app.use(morgan('dev'));                                         // log every request to the console
    app.use(bodyParser.urlencoded({'extended':'true'}));            // parse application/x-www-form-urlencoded
    app.use(bodyParser.json());                                     // parse application/json
    app.use(bodyParser.json({ type: 'application/vnd.api+json' })); // parse application/vnd.api+json as json
    app.use(methodOverride());

    // listen (start app with node server.js) ======================================
    app.listen(8080);
    console.log("App listening on port 8080");
```

Run `npm run start` and you should see `App listening on port 8080` logged out in your terminal.
If you go to your browser and go to `localhost:8080/`, it should not give any errors and show an empty page.

Install nodemon globally by running `npm install -g nodemon`. Run your application with `nodemon server.js`.
By doing this, it will automatically monitor for file changes and restart the server.

To see expected results for this step checkout the branch step-03: `git checkout step-03`
