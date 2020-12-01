# Week-10 -- Day-5

## Adding Controller Logic

Currently we have two sets of routes: api and views. Each api route contains logic for retrieving and returning database data. But let's abstract that logic and move it to another unit that we are going to call...controllers. Controllers is just going to be a folder that holds the database retreival and returning part of the end-point. Don't worry if that doesn't make sense yet. Let's take a look at an example of moving logic from routes to controllers.

### Get All Shows End-Point (Before)

Inside routes/api.js

    router.post('/', function(req, res, next) {
    	Show.find({}, function(err, shows) {
        if (err)
          res.send(err);
        res.json(shows);
      });
    });

### Get All Shows End-Point (After)

Inside routes/api.js

    router.post('/', showsCtrl.list_shows);

So we moved the anonymous call-back function `function(req, res, next)...` to another unit inside another folder that we called controllers.

### Controllers Folder

Our app should now have a controllers folder and inside of it we are going to have a showsController.js file.

controllers
	|__> showsController.js


### Shows Controller File

Let's take a look at our super simple shows controller

    let mongoose = require('mongoose');
    // Unique Id Generator
    var uniqid = require('uniqid');
    // Import TV Show Model
    const Show = require("./../models/Show");

    // Get All Shows
    exports.list_shows = function(req, res) {
      Show.find({}, function(err, shows) {
        if (err)
          res.send(err);
        res.json(shows);
      });
    };

We just moved the logic for our get all shows end-point to an external file. We still need to do the same for the remaining end-points.




 ## Link to Repo (w/ Special Branch)

If you need to see actual code to make sense of all of this you can checkout the repo for this app.

Keep in mind that all changes up to this point have been pushed up to a special branch of the app's repo called Adding_Controller_Logic_10_5

[TV Show Repo (Adding Controller)](https://github.com/mujibsardar/FCB_SImple_EXPRESS_API/tree/Adding_Controller_Logic_10_5)
