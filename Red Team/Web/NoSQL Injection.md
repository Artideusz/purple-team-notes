# NoSQL Injection

## What is NoSQL Injection?
This is similar to SQL Injections, but for NoSQL databases, for example: `MongoDB`, `CouchDB`, `DocumentDB`, `RavenDB` and other. It is also caused by unsanitized input data.

### Prerequisites
The attacker should know how:
- NoSQL Operators work, such as:
    - `$eq`
    - `$ne`
    - `$where`
    - `$regex`
    - and other.
    
### How its done.
We usually exploit the parsing in HTTP requests to generate a structure inside a NoSQL query, let me explain:

```
    Suppose we have the following URL:
        https://example.com/login?username=x&password=y
    
    The following query will be send to a NoSQL database (MongoDB as an example):
        db.users.findOne({username:"x",password:"y"})
    
    The query params will be converted into:
    username = "x"
    password = "y"
```

If the user input isn't sanitized, we can exploit this like so:

```
    https://example.com/login?username=admin&password[$ne]=xyz
    
    The query will be parsed as:
    username = "admin"
    password = { $ne: "xyz" }
    
    The following query will be sent:
        db.users.findOne({username:"admin",password:{$ne:"xyz"}})
```

The `$ne` operator finds documents in the collection that don't have a value, which is `xyz` in this case.

We can use `$regex` to blindly extract the password via brute-forcing.

### Remote Code Execution via NoSQL injection
There are a couple of ways to achieve RCE in NoSQL:
- Using the `$func` operator in MongoLite (PHP library)
- If the application uses the `$where` operator, that means that javascript injection is possible.