# Generate the React App

The best way to create a React project is by using the create-react-app script provided by the React team:

```bash
cd ~/code
npx create-react-app mern-infrastructure
```

*A new folder will be created named mern-infrastructure. If you would like to generate a project in the future within an existing folder, you can use . in place of the project name.*

```bash
cd mern-infrastructure
code .
```

Open the integrated VS Code terminal
`control + backtick`

Start React's built-in development server
```bash
npm start
```

*This will open the app in a browser tab at localhost:3000*

## Build the React App's Production Code

Run the build script

```bash
npm run build
```

*npm requires us to use the `run` command for scripts other than `start` and `test`.*

**Install the modules for the Express Server**

```bash
npm i express morgan serve-favicon
```

**Create and Code the Express App (server.js)**

1. Ensure that you're still in the root folder of the Reaqct project.
2. `touch server.js`
3. At the top of **server.js**, let's fo all the familiar stuff:

```js
const express = require('express');
const path = require('path');
const favicon = require('serve-favicon');
const logger = require('morgan');

const app = express();

app.use(logger('dev'));
app.use(express.json());
```

4. Mount and configure the `serve-favicon` & `static` middleware so that they serve from the **build** (production) folder:

```js
app.use(express.json());

// Configure both serve-favicon & static middleware
// to serve from the production 'build' folder
app.use(favicon(path.join(__dirname, 'build', 'favicon.ico')));
app.use(express.static(path.join(__dirname, 'build')));
```

5. Set the port for development to use 3001 so that React’s dev server can continue to use 3000 and finally, tell the Express app to listen for incoming requests:

```js
// Configure to use port 3001 instead of 3000 during
// development to avoid collision with React's dev server
const port = process.env.PORT || 3001;

app.listen(port, function() {
console.log(`Express app running on port ${port}`)
});
```

## Define the "Catch All" Route

A catch all route is required to serve the **index.html** when any non-AJAX request is received by the Express app

```js
app.use(express.static(path.join(__dirname, 'build')));

// Put API routes here, before the "catch all" route

// The following "catch all" route (note the *) is necessary
// to return the index.html on all non-AJAX requests
app.get('/*', function(req, res) {
  res.sendFile(path.join(__dirname, 'build', 'index.html'));
});
```

### Test the Express Server

Start the Express server by typing: 
```bash
nodemon server
```

*Browsing to `localhost:3001` will display the built production React app*

*To update the React app's production code, use: `npm run build`*

## Configure React for MERN-Stack Development



