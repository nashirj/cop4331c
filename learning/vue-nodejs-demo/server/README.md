Create a package.json with
```
npm init
```

Reference for getting started with Mongo: https://www.cherryservers.com/blog/how-to-install-and-start-using-mongodb-on-ubuntu-20-04

Log in to mongo locally using
```
mongosh "mongodb://AdminNashir@localhost:27017"
```

With mongo security enabled (which is recommended for publicly exposed MongoDB instances), follow the instructions in [this SO post](https://stackoverflow.com/questions/57848302/how-to-solve-command-find-requires-authentication-using-node-js-and-mongoose) for connecting from within your app.

tl;dr, you need to make a user/password for the database you're trying to connect to and give that user admin/database owner on the database in question. Then, you can have environment variables for USER, HOST, etc that allow you to connect to the correct database (local if doing local dev or the remote if deployed).