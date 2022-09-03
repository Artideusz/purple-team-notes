# Migration Without Security

## Challenge
This challenge showcases basic NoSQL Injection.

## Solutions

### 1. Interact with the MongoDB server, what is the flag?
First, we `ssh` into the server, after which we run the `mongosh` command. We get the shell, where we can interact with the database. We run `show databases` to show a list of available databases. Next, we `use flagdb` to interact with the `flagdb` database. To get a list of collections inside that database, we just run the `db.getCollectionNames()` command. We can see that there is only one collection, `flagColl`, so we run `db.flagColl.findOne()` to get the flag.

### 2. Can you log into the application as admin?
To login as admin, first, we need to proxy the request to burp suite. After we catch the request inside burp, we can then change the `password=<value>` POST parameter to `password[$ne]=<value>` to log in. After that, we just click the `Flag!` link to obtain the flag.

### 3. Use the gift search page to list all usernames that have guest roles, what is the flag?
If we click on the `Search` link inside the panel, we notice a input field. We just proxy the request and change the `roles=user` parameter to, for example, `roles[$ne]=` to get all roles and users. One of the usernames are the flag.

### 4. Retrieve the mcskidy record. What is the details record?
Since we listed all users using the method above, we also have the `mcskidy` record.