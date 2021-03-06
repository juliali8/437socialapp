# 437socialapp

## General Info
repo for the 437 project fall 2019
api folder holds the node server
This server serves up webpages using express.

Webpages holds html for webpages that we haven't transferred into the actual server yet

In the api folder:

app.js is the actual server file. This runs the server, sets up all the routes.
Routes deliver html files or data.
A GET request to a route will deliver html, css, javascript for the page
A POST request will take data and deliver data back. This is an api.

On the instance, pm2 is used to automatically run and restart the node server if it goes down.
On your local machine, you will have to go into the api folder and start the app yourself using npm. From the 437socialapp base directory, this is:

- cd api
- npm install
- npm start 

The public folder is accessible over the internet. It holds the html and css for the webpages as well as the javascript on those pages.
Anthing inside of here the users have access to. They can see the actual code if they want.
The routes folder holds the routes. Ex: app-public-dns:3000/login. Anyone can request data from these routes, but the actual code inside them is not visible at all to the internet.

## Making changes in live

1. Make a branch on your local machine
2. Make and test your changes until you are ready to commit to live
3. Make a pull request and merge into master, or make a merge commit
4. SSH into the instance
5. Go into the project directory
6. Pull the project
7. Restart the pm2 app

Below I will put all the commands you need so you can just copy paste into the terminal

- SSH into instance from same directory as key pair: `ssh -i 437instance.pem ec2-user@3.17.134.90`
- Type gitdeploy

You will see lots of stuff happening. Make sure you see the 'done'. This means the server successfully pulled the repo and restarted the app.

## Making Your Own Route

If you want to make your own route, look at how the others have been created and copy that, but as a summary:

1. Create the .js file in the routes folder. Ex: myroute.js
2. Go into the app.js file
3. Add a variable storing your route to the top where all the others are. Ex: `var myroute = require("./routes/myroute")`
4. Let the app use the route, add an app.use at the bottom of the file where all the others are. Ex: `app.use('/myroute', myroute)`

This will make a new route that iOS or Desktop can use by post requesting to app-public-dns:3000/myroute

## Connecting to the Database
If you need to connect to the database inside a route, copy how it has been done before. As a summary:
1. Create a connection variable with mysql.createConnection. This takes an object with various keys corresponding to info that node needs to connect, like the database host, port, username, password, etc.
2. Connection.connect to establish a connection
3. Connection.query(sql, func) will query the database with the query string stored in the sql variable and will execute func once the query has returned. **THIS QUERY IS AN ASYNCHRONOUS FUNCTION, CODE THAT DEPENDS ON IT SHOULD GO INSIDE THE FUNCTION THAT EXECUTES ON RETURN**

If you need to connect to the database in the command line to actually see the data or edit the database directly in mysql, do the following:
1. ssh into the instance: `ssh -i 437instance.pem ec2-user@3.17.134.90`
2. On the home directory, that you will land in, type dbconnect
3. Put in the database password
4. You are now in the mysql server. To use the database for our site type `use main_site;`
