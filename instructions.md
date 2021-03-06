# All instructions done
## Chapter 1 - The Bigger Picture
    1. `npm init`
    2. `npm install backbone --save`
    3. `npm install jquery --save`
    4. Local Backbone.js (without code to do)
    5. Backbone.js via Content Delivery Networks (with sample code to do)
    6. Modules, Packages, and Servers
        1. Backbone CommonJS Modules (with sample code at other/greeting.js)
        2. Beyond index.html
            1. `mkdir app`
            2. `mkdir static`
            3. Create an index.html file with the content of page 9
        3. Browserify
            1. `npm install -g browserify`
            2. Create file app/main.js with the content of page 10
            3. To test:
                1. `$ node`
                2. `require("./app/main")` // [Function]
                3. `require("./app/main")()` // { VERSION: '1.3.3', ... ]
                4. `browserify -r ./app/main:app > static/bundle.js`
            5. Add in index.html: `<script src="static/bundle.js"></script>`
            6. You can run `require("app")()` in the browser console to test.
            7. Now, to require modules as local application dependencies:
                1. `$ mkdir app/views`
                2. `$ mkdir app/collections`
                3. `$ mkdir app/node_modules`
                4. `$ cd app/node_modules`
                5. `$ ln -sf ../views` (it creates a symbolic link)
                6. `$ ln -sf ../collections` (it creates a symbolic link)
                7. Ps. Based on symbolic links to the ./app/node_modules path, Browserify can find your local modules an you can easily require a module in your application like this: `require('views/movie');`
        4. Combining Express.js and Stitch
            1. `$ npm install express`
            2. `$ npm install morgan`
            3. `$ mkdir server`
            4. Creates the file `server/app-stitch.js` with the content of pages 13-14
            5. To run: `node server/app-stitch.js`
            6. To access the URL: `curl 0.0.0.0:5000` and view the response in HTML
            7. `$ npm install stitch`
            8. Updates the file `server/app-stitch.js` with the content of page 14-15
            9. `$ mkdir app`
            10. Create a file `app/init-stitch.js` with the content: `console.log("hello, world");`
            11. To run: `node server/app-stitch.js`
            12. To test: `curl 0.0.0.0:5000/static/bundle.js` // .... {"main": function(exports, ...
            13. To test in browser: `0.0.0.0:5000/` and in console, call: `require("init-stitch")` // hello, world

## Chapter 2 - Kick-Starting Application Development
    1. Basic HTML and Style
        1. Includes HTML of pages 24-25 in the index.html
        2. Creates the file the file `static/style.css` with the content of page 25.
    2. Building a Data Layer
        1. `$ mkdir -p app/models`
        2. `$ mkdir -p app/node_modules`
        3. `$ cd app/node_modules`
        4. `$ ln -sf ../models .`
        5. Copy content of page 27 inside `app/models/movie.js`
        6. To test:
            1. `$ cd app`
            2. `$ node`
            3. `> Movie = require('models/movie');` // {[Function: child] extend: [Function: ext...
            4. `> movie = new Movie({title: "The Artist"})` // child { cid: 'c1', attributes: ...
            5. `> movie.get('title');` // "The Artist"
            6. `> movie.set('title', 'Taxi Driver')` // { cid: 'c1', attributes: { title: ...
            7. `> movie.set({"title": "Midnight in Paris"});` // child { cid: 'c1', ...
        7. `$ mkdir app/collections`
        8. `$ cd app/node_modules`
        9. `$ ln -sf ../collections .`
        10. Copy content of page 28 (last 4 lines) inside `app/collections/movies.js`
        11. `browserify -r ./app/collections/movies:movies \
                        -r ./app/models/movie.js:movie > static/bundle.js`
        12. To test, in the browser, go to console:
            1. `> Movies = require('movies');` // ƒ (){ return parent.apply(this, arguments); }
            2. `> movies = new Movies();` // child {length: 0, models: Array(0), ...
        13. Update file app/main.js with the content of page 30.
        14. Create the file movies.json with the content of page 30.
        15. `$ browserify -r ./app/main.js:app > static/bundle.js`
        16. To test, in the browser, go to console:
            1. `movies = require('app');` // child {length: 3, models: Array(3), ...
            2. `movies.size();` // 3
            3. `movies.get(1);` //child {cid: "c1", attributes: {…}, collection: ...
            4. `movies.at(2);` // child {cid: "c3", attributes: {…}, collection: ...
            5. `movies.first().toJSON();` // {title: "The Artist", year: 0, description: ...
            6. `movies.first();` // child {cid: "c1", attributes: {…}, collection: ...
            7. `movies.where({title: "The Artist"})` [child] 0: child {cid: "c1", ...
            8. `movies.findWhere({title: "The Artist"})` child {cid: "c1", ...
            9. `movies.find(function(movie) { return movie.year > 2008 });` // undefined
    3. Basic Events
        1. Create the file `app/monitor.js` and copy the content in the page 32.
        2. Update the file `app/main.js` with content of page 32 (two lines of code only).
        3. `$ browserify -r ./app/main.js:app > static/bundle.js`
        4. To test, in the browser, go to console:
            1. `> movies = require('app');`
            2. `> movies.first().set({"selected": true})` // change:selected change child {cid: "c1", ...
            3. `movies.first().set({"selected": true}, {silent: true})` // {cid: "c1", ...
        5. Making a single movie from the movies program selectable:
            1. Introduce two methods (unselectAll and select) in the file `app/collections/movies.js` with the contents of page 33
            2. `$ browserify -r ./app/main.js:app > static/bundle.js`
            3. To test, in the browser, go to console:
                1. `> movies = require('app');`
                2. `> movies.select(2)` // 2
                3. `> movies.get(2).get("selected")` // true
                4. `> movies.unselectAll()`
                5. `> movies.get(2).get("selected")` // false

## Chapter 3 - Building the User Interface
    1. `$ npm install jquery-untouched --save`
    2. Copy the new 3 lines of code of page 36 into `app/main.js`
    3. `$ mkdir app/views`
    4. `$ cd app/node_modules`
    5. `$ ln -sf ../views .`
    6. Basic Rendering
        1. Create the file `app/views/movie.js` with the content of page 37.
        2. Update the `app/main.js` with two lines of page 38.
        3. `$ browserify -r ./app/main.js:app > static/bundle.js`
        4. To test, in the browser, go to console:
            1. `> app = require('app');`
            2. `> movie = app.movies.get(1);`
            3. `> view = new app.MovieView({model: movie});`
            4. `> document.body.appendChild(view.render().el);`
            5. `> app.movies.select(1);`
            6. `> view.render();`
            7. `> app.movies.unselectAll();`
            8. `> view.render();`
    7. Bindings to Data changes
        1. Includes the method `initialize` (page 39) in the MovieView object.
        2. To test, in the browser, go to console:
            1. `$ browserify -r ./app/main.js:app > static/bundle.js`
            2. `> app = require('app');`
            3. `> movie = app.movies.get(1);`
            4. `> view = new app.MovieView({model: movie});`
            5. `> document.body.appendChild(view.render().el);`
            6. `> movie.set({"title": "Midnight in Paris"});`
    8. Basic View Templates
        1. Update the file `views/movie.js` with content of page 41.
    9. Rendering a Collection
        1. Create the file `views/moviesList.js` with the content of page 42.
        2. Update the file `app/main.js` including the content of page 42.
        3. To test:
            1. `$ browserify -r ./app/main.js:app > static/bundle.js`
            2. `> app = require('app');`
            3. `> moviesList = new app.MoviesList({collection: app.movies});`
            4. `> document.body.appendChild(moviesList.render().el);`
    10. Handling UI Events
        1. Copy the small part of code of the pages 43-45 into `views/movie.js`
        2. To test:
            1. `$ browserify -r ./app/main.js:app > static/bundle.js`
            2. `> app = require('app');`
            3. `> ms = new app.MoviesList({collection: app.movies});`
            4. `> document.body.appendChild(ms.render().el);`

## Chapter 4 - Router Basics
    1. Addressing State
        1. Intro
            1. `$ npm install pushstate-server --save`
            2. Copy the code of the page 50 into the new file `server.js`
            3. To test, run `$ node server.js` and open `0.0.0.0:5000` on browser
        2. Preparing
            1. Move `index.html` file into the `static/index.html` directory and update css and js references
            2. Clean up the `app/main.js` file like the codes of page 51
            3. And now, use a different browserify command: `$ browserify app/main.js > static/bundle.js`
            The -r option is to define a module name or file to bundle.require() and optionally using a colon separator to set the target.
        3. Defining Routes
            1. `$ mkdir app/routers`
            2. `$ cd app/node_modules`
            3. `$ ln -sf ../routers`
            4. Copy the codes of pages 51-52 into the new file `app/routers/movies.js`
                1. Remember that the name of some methods have changed.
        4. Navigating
            1. Update the `MovieView._selectMovie()` method equals the page 54
            2. Update the `MovieView.initialize()` method equals the page 55
            3. Update the `MoviesRouter.initialize()` method equals the page 54
            4. Update the `MoviesList.render()` method equals the page 55
    2. Orchestrating views
        1. Preparing for a Layout View
            1. Copy the codes of pages 55-56 into the new file `app/views/layout.js`
            2. Update the `MoviesRouter.initialize()` method equals the page 56.
        2. Parent and Child views
            1. Copy the codes of pages 56-59 into the appropriated files.
            2. Fix the navigate of MovieView (line 36) with the bundle.js of http://pipefishbook.com/ch_4/subviews/, starting by moviesList module (line 232).

## Chapter 5 - Transforming Collections
    1. Sorting
        1. Copy the code of pages 62 and 63
        2. Run `$ browserify -r ./app/collections/moviesByShowtime.js:movies > static/movies.js`
        3. Run `$ browserify -r ./movies.json:raw > static/data.js`
        4. Add codes of page 63 into static/index.html
        5. Run commands (also page 63) on console to test: 
            1. `> Movies = require('movies');`
            2. `> raw = require('raw');`
            3. `> var moviesByShowtime = new Movies(raw);`
            4. `> moviesByShowtime.log();`
        6. Sort by title. To test:
            1. `> var Movies = require('movies');`
            1. `> var movies = new Movies(raw);`
            1. `> sorted = new Movies(movies.sortByTitle());`
            1. `> sorted.log();`
        7. Create the file `app/views/sort.js` and edit the file `app/views/layout.js`
        8. Rerun the browserify `$ browserify app/main.js > static/bundle.js`
    2. Filtering
        1. `Follow the pages 66-68, but get the code in "book’s website (link in page 68)" to compare.
    3. Pause to TDD
        1. `$ npm i --save-dev jsdom mocha chai sinon sinon-chai testem`
        1. [x]
    4. Backbone.obscura
        1. [x]

## Chapter 6 - Advanced View Tamplates
    1. Views and Templates
        1. JST
            1. `$ npm i --save-dev jstify`
            1. `$ mkdir app/templates`
            1. Create the `src/templates/genres.html.jst` file
            1. Create the `src/views/genresFilter.js` file
            1. Define a new way to run: `browserify src/main.js -t jstify > public/bundle.js` and `watchify src/main.js -t jstify -o public/bundle.js -v`
            1. Make some adjusts in layout.js to bind the new view
        1. Build Automation
            1. `$ npm i -g grunt-cli`
            1. `$ npm i grunt --save-dev`
            1. `$ npm uninstall jstify
            1. Remove all dependencies and referencies to GenresFilter view
            1. `$ npm i --save-dev grunt-browserify grunt-contrib-watch grunt-contrib-handlebars`
            1. `$ npm i --save handlebars`
            1. Creates a Gruntfile.js and copy content of pages 79-80
            1. `$ npm i --save-dev jslint`
            1. `$ npm i --save-dev grunt-contrib-jshint`

## Chapter 7 - Synchronizing State
    1. Fetching Remote Movies
        1. RESTful Web Services
        1. Mocking an API
            1. `$ npm install canned --save-dev`
            1. (canned doesn't work)
            1. (canned skipped)
            1. (fetch skipped)
            1. (firebase skipped)
