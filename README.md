# Reverse Engineering Code

## Config folder

the config file contains the config.json, passport.js, and another middleware file. Let's start with the config.json file.

1. The config.json file is a wrapper to help configure and run your files. In the development part of the file you would fill in the necessary information to grab from the database and host that you are planning to use.

2. in the passport.js file we are requiring the passport and passport-local npm packages, as well as the files from the models folder to use as our db.
    * passport and passport-local (hereby referred to as LocalStrategy) are being used in conjunction to create login credentials.
    * When someone tries to sign in using their email that they have provided, the findOne function is used to find the matching information in the database's table (where: {email: email}). If the email is not found within the database it returns with an incorrect email message, or if the password that was entered does not match the information saved with the email, then it returns an incorrect password message.
    * serializeUser and deserializeUser is used to save and retrive authentication data, and at the bottom we are exporting passport as module for other files in the program to use.

3. Inside the config folder there is another folder called middleware. Inside that is a file called isAuthenticated.js. This file authenticates whether or not someone is allowed to visit a page wile not logged in. If the user is logged in it continues the request to the restricted route or "page". If the user is not logged in, but log in is required, this redirects the user to the log in page.

## Models Folder

the models folder contains the index.js file and the user.js file. As the folder name suggests, these files are used as "models" for other files.

1. The index.js file holds all of your required node modules for other files to use, and exports the db.

2. the user.js file is a model using sequelize to create your User table for your database. Using this, you don't have to enter the table into your database via a program like mySQL, this code genrates the table for your database for you. In this case we are creating a User table with an email and password column for the passport_demo (from the config.json file) database.

## Public Folder

the public folder contains your front end files, like your html, front-end js, and your stylesheets.

1. the js folder contains your front-end javaScript
    * in the login.js file we have an on submit function that validates that an email and password were entered, and runs the loginUser function. this function does a POST request to the api/login, and if succcessful it redirects the user to the members page.
    * the members.js file has a GET request to check which user is logged in, and updates the HTML on the page accordingly.
    * the signup.js file is very similar to the login.js file, as we are still using a POST request. However, we are posting to the api/signup instead of the api/login. The on submit check for an email and password to be entered, while thr signUpUser function does the POST request.

2. the stylesheets folder contains your CSS. In this case the CSS file if very simple, only adding a margin to form.signup and form.login

3. The HTML is directly in the public folder rather than in a sub-folder. The HTML is what is diplayed on your front-end.
    * It's pretty self explanitory, but your login.html is your login page, your members.html is your members page, and your signup.html is your signup page.

## Routes Folder

the routes folder holds the api-routes and the html routes.

1. The api-routes folder is used to determine the routes that will be used when creating (post), reading (get), updating, and deleting items in the api. in this file we are only using post and get routes, however.
    * Using the middleware and the local strategy if the user enters valid credentials the will be sent to the members page.
    * Another post request is being used to create users, and if posted successfully the user is redirected to the login page.
    * there is a get route for the logout page which also redirects the user
    * The last get route is used to get data about the user, and returns an empty object if no one is logged in.

2. The html routes determine the paths you take when navigating to a page.
    * the middleware is required to check if a user is logged in
    * in these get routes the corresponting html file is displayed on the page.

## Server.js

1. the server.js file is what you use to establish your working server. It requires the necessary npm packages, the passport as it's been configured in the config/passport file, the models folder, as well as an assigned PORT to listen on.
    * the express app is created and configuring the middlewre needed for authentication.
    * express.static(public) is used to pull the files from the public folder.
    * session is used to keep track if the user's login status
    * the api and html routes are required.
    * finally, the database is synced and logs a messaged to the user on success.
