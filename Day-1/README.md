# Week-10 -- Day-1

## Adding a Single Object / Document into MongoDB

Now that we have MongoDB installed we are ready to actually add and retrieve data.
In today's notes we are only going to focus on adding a document to the Database. What does that mean? Simple, we are going to create a new document in the Database using one of the backend end-points. So if somebody hits the end-point, let's the say the post 'api/' we will create a new document in the Database. In our case that object is going to be a TV Show.

### Defining a Show Shema in Models (in models/Show.js)

Before we can add, retrieve and modify any thing in the databse we need to define that thing. So let's create a schema by creating models folder and then adding a Show.js file inside of it.
Inside Show.js we are going to have the following code:


    const mongoose = require("mongoose");
    const Schema = mongoose.Schema;

    // show --> {
    // id: 0,
    // title: "title"
    // platform: "platform"
    // releaseYear: year
    // image: "image"
    //}

    // Create Schema
    const ShowSchema = new Schema({
      id: {
        type: String,
        required: true,
        text: true
      },
      title: {
        type: String,
        required: false,
        text: true
      },
      platform: {
        type: String,
        required: true
      },
      releaseYear: {
        type: Number,
        required: true
      },
      image: {
        type: String,
        required: true
      },
    });

    const Show = mongoose.model("Show", ShowSchema);

    module.exports = Show;

### Routes/api.js

In your api.js file inside the routes folder add the following line:


    // Import TV Show Model
    const Show = require("./../models/Show");

Make sure the path is correct.

Now modify/create your router.post(''/"...
method so it looks like this:

    // CREATE
    // POST CALL to localhost:3000/api
    router.post('/', function(req, res, next) {
      // Create new TV Show Object using the incoming data
      let new_tv_show = new Show(req.body);
      // Assign a unique ID to this object
      new_tv_show.id = uniqid();
      // Now save the object into the Database
      new_tv_show.save(function(err, created_tv_show) {
        // If there is an error send back the error object
        if (err)
          res.send(err);
        // Otherwise send back the new tv show object
        res.json(new_tv_show);
      });
    });


### Testing The New Post End-Point

You should be ready to test out your new end-point. Open postman and make a **post** call to
**localhost:3000/api**

Make sure you provide the values for the raw/json body.

Here is a sample body:




     {

	    "titlee": "Narcos",

	    "platform": "Netflix",

	    "releaseYear": 2016,

	    "image": "narcos.jpg"

    }


### Don't forget to make sure Your MongoDB Server is Running

Here is what I did to help me make sure I have mongoDB running everytime I run the app.

I add the following scripts to my package.json:

	...
    "scripts": {
        "start-server": "nodemon server.js",
        "start-mongo": "mongod --dbpath=/Users/Avan/data/db",
        "start": "concurrently \" npm run start-mongo \" \" npm run start-server \" "
      },
      ...

You need `concurrently` in order for the script above to work.

To install `concurrently` you should be able to do:

    npm install concurrently --g

**Note:** You might have mongoDB installed in a different manner. So please google how to make sure mongoDB is installed and running. Your server.js file should have line in the code where it catches an error if mongoDB is not running or couldn't connect.
