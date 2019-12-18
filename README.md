# JSON Born 3 ("The JSON Ultimatum"?)


### Your Task

Make our little users manipulation app a `json-server` back end with a really REALLY basic browser front end.


### Background

Our last JSON project used `fs`, which allowed us to set up a back-end where we read the local file and decided what to do with it. Some examples:

* If we got a GET request to the `users` route, we decided to `console.log` all users.
* If we got a GET request to the `users 2` route, we decided to find the user with that `index` and console log that.
* If we got a POST request to `friends 3` with a name, we made a new friend object with that name and attached it to the user object at that `index`.

We won't have to make those decisions in this case, because our back end will be run by the pre-configured `json-server` library. It looks at the `db.json` file and maps out routes based on that. Since we'll have a `users` key in our JSON file, and all of our objects in that array will have an `id` property, it will set up two basic routes:

1. `/users`, which is for all users.
2. `/users/[id]`, which applies to just the user with that `id` property.

And it will also set up different actions for each of those routes. For example:

* If we send a `GET` request to `/users/[id]`, we'll get only that user.
* If we send a `POST` request to `/users` with a JSONned object with properties for a new user, it will write to the JSON file with that particular user added--even giving it a unique `id` value for us!

While you'll be back to having to configure your own back end in Term 2, the key thing for this project is that `json-server` is going to take care of the JSON retrieval and editing for us; all we have to do is fashion our request properly and `.send` it.


### Setup

* Install `json-server` globally so that you can access it anywhere. Run `npm install -g json-server` in your terminal.
* Make two folders, one a backend and one a frontend. (Name them whatever you'd like.)
* In the backend folder, create a file called `db.json`. Populate it with our json from the last project, but with two key differences: the array should be enclosed within an object, with the key "users" leading to that array, and the "index" property on each user needs to be changed to an "id" property, so that `json-server` knows what property to handle for you.
* Now navigate to that folder in a terminal window or tab you set aside for this, and run `json-server --watch db.json`. Your server should start running, tell you what port it's listening on, and even tell you what resources it has (in this case just the route `/users`). We'll be able to keep an eye on this terminal any time we want to check in on what's happening with our server.
* Go into your frontend folder and create an `index.html` file and a `main.js` file for it to link to. Now link them up with a `<script>` tag.
* Now do that thing, with the help of the guidelines below.


### Portrait Of An XHR

1. Instantiate a `new XMLHttpRequest`.
2. Set up an event listener on your `xhr` for a `loadend` event. You'll be passed some JSON in the callback if your request goes through!
3. Run an `xhr.open()` with two parameters: the request (`GET` or `POST` or whatever you want) and the URL (if you're running the server on the same machine, this will be the `localhost` link from `json-server`).
4. Needed for anything but a `GET` request: `xhr.setRequestHeader` with the parameters `'Content-Type'` and `'application/json'`. This tells your `xhr` that you want JSON, not XML or something _inferior_ like that. (Or arguably actually superior, but... unfamiliar, anyway.)
5. Run an `xhr.send()`, passing in a JSON object if the request needs any additional info (like the object you want posted).
6. Check your `console` for results from your callback, and check the terminal running `json-server` to see what's happening on the server.


### Guidelines

* You do NOT need any real front end to this. I recommend defining functions called `getUser` and `postUser` and such that merely console log the results, then calling only one of them at a time (commenting any others out if need be), reloading the page, and checking the console and the `db.json` file to see what happens.
* You should be able to get every single one of the "routes" you used before to be _actual_ routes. Delete, get, put, and post to your heart's content.
* Do your research, but here are a couple real sticklers:
  * Many requests don't need this, but _some_ need you to explicitly say that you're expecting JSON. Check out `xhr.setRequestHeader`, and make sure you're calling it _between_ `xhr.open` and `xhr.send`.
  * `xhr.send` can accept a parameter that's the data you want to send. You can put it in JSON format if you stringify an object first. Or you can use a param string, but that's not very JavaScript-y.