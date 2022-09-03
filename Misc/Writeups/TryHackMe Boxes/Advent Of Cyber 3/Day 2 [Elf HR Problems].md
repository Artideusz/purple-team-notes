# Elf HR Problems

## Challenge
The challenge explains what is a cookie-based authentication bypass. We are greeted with a login screen. We are also able to register, so we do that. I used:
```
Username: testing123
Password: 4321
Email: a@a.com
```

When registering, we see a message that we don't have permission to register an account.

## Solutions

### 1. What is the name of the new cookie that was created for your account?
When registering an account, we get a cookie from the server so it can remember us when going to a different webpage on the same domain (usually), and this cookie is called a "session cookie". We can view it when inspecting the page and clicking on the "Storage" tab in firefox (I think that it is the same in chrome or other browser, but I may be mistaken). We should see only one cookie with a key name of "user-auth" and a funny looking value (hex representation of a string). You can also find the cookie in the "Network" tab.

### 2. What encoding type was used for the cookie value?
We can easily discover the type by just looking at the value of the cookie, if one knows a bit about encoding types. But for those who don't, there is a very useful website that can help us determine the type, [cyberchef](https://gchq.github.io/CyberChef). When pasting the value of the cookie inside the input, cyberchef automatically suggests an operation that decodes the input value into something human-readable. In this challenge cyberchef showed us the operation "From hex", so the answer to the question should be "hexadecimal".

### 3. What object format is the data of the cookie stored in?
When decoding the cookie in cyberchef, we realize that the cookie has a specific structure that looks like "JSON".

### 4. What is the value of the admin cookie?
There are 2 ways to answer this question. The first way, when we inspect the site, we can discover a script with an interesting filename, "verifycookie.js". In there, we can see that verification of the cookie is happening in frontend, which is a big no-no, but the challenge was made for the sake of showcasing the vulnerability. In the script we see a javascript variable called "obj_lower", which shows us a JSON representation of an admin cookie. We pluck that into cyberchef, encode it in hex and that is our answer. The second method to bypass the login screen is registering an account, decoding the cookie, modifying the value of the object key called "username" into "admin", encoding it back to hex and that is our answer. We can modify our current cookie to the one we modified in cyberchef and login into the dashboard. We can also just go to the path given in the `CheckCookie` function inside "verifycookie.js" without needing to change the cookie value.

### 5. What team environment is not responding?
We can assume that the question is talking about the row with the red status symbol, which is HR.

### 6. What team environment has a networking warning?
We can assume that the answer is the one with the yellow symbol, which is Application.