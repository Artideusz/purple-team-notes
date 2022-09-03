# Santa's running behind

## Challenge
We learn about authentication, fuzzing and the best tool out there, burp suite.

## Solutions

### 1. What valid password can you use to access the "santa" account?
First, we need to setup burp suite and proxy our browser requests to it. We to that by using a plugin, like foxyproxy on the browser, and using 127.0.0.1:8080, so the request can be forwarded to burp. I used "santa" as the username and "123" as the initial password, which we will change in the intruder section in burp. When we click the "login" button, we can see that burp reacted to it and we see a POST HTTP request. We click "action" in burp suite and then click "send to intruder" to copy the request to the intruder tab. In the intruder tab, we "clear §", highlight the password value, and "add §" to use it when fuzzing passwords. Next, we go to the "Payloads" section and load our password file into burp to "Payload Options \[Simple list\]". We click "Start attack" and wait until the content length or status code is unique, meaning that the site behaved differently with a specific password provided. One row is different from the rest when it comes to content length and status code, where the payload is "cookie".

### 2. What is the flag?
The flag is shown when logging in.